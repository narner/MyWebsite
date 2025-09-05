```
title: "Building Swift Bindings for Chroma"
date: "2025-09-05"
```

I recently collaborated with the folks at [Chroma](http://trychroma.com) (a company I'm an investor in) to build [**Chroma Swift**](https://github.com/chroma-core/chroma-swift) — a Swift package that brings Chroma’s retrieval system to iOS and macOS applications. This post outlines the technical journey of bridging Chroma’s Rust core with Swift, including cross-platform compilation, embedding support, and cloud synchronization.

Until recently, most developers building apps with embeddings and vector search relied on remote APIs (e.g. OpenAI, Cohere) to generate embeddings and store them in cloud databases. That approach works, but it has drawbacks: network latency, recurring costs, and potential privacy concerns.

At the same time, Apple Silicon and the [MLX](https://github.com/ml-explore/mlx-swift) ecosystem have unlocked fast, lightweight model inference directly on iOS and macOS devices. This means we can now **generate embeddings locally**, store them in a retrieval system that runs on-device, and query them instantly — without ever sending user data off the device.

Chroma Swift brings these pieces together:

- **Chroma’s embedded Rust core**, compiled into an XCFramework
- **Type-safe Swift bindings** for easy integration in iOS/macOS apps
- **On-device embeddings via MLX**, with multiple models to trade off speed vs. quality
- **Flexible storage options** (ephemeral, persistent, or cloud-synced)

Running models on-device for inference has been possible for some time, but with Chroma Swift you also get **retrieval built in**. You can generate embeddings locally and then **store, search, and query them directly on-device**. 

------

## **Project Structure**

There are 2 components to building ChromaSwift, the code for generating the bindings, and the Swift package itself.

At a high level, Mozilla’s [UniFFI](https://github.com/mozilla/uniffi-rs) was used to generate Swift bindings, a cross-platform (XCFramework) was created out of the Chroma Rust library, and higher level Embeddings API that leverages MLX was authored. 



### **Rust FFI Layer** 

- `lib.rs` — Core FFI implementation via UniFFI
- `Cargo.toml` — Dependencies on Chroma crates + UniFFI
- `build_swift_package.sh` — Cross-compilation + packaging script
- `uniffi-bindgen.rs` — Binding generator entry point

This code currently resides on this [pull-request](https://github.com/chroma-core/chroma/pulls?q=is%3Apr+is%3Aopen+swift+), which will eventually be incorporated into the main Chroma database.



### **Swift Package** 

* Auto-generated Swift bindings

- ChromaEmbedder.swift — High-level API for local embeddings
- Four demo apps showing real-world usage



The Swift package resides [here](chroma-core/chroma-swift: Run Chro…), and can be added to your Swift project by adding this to your project's `package.swift`:

`.package(url: "https://github.com/chroma-core/chroma-swift", from: "1.0.0")`

------





## **UniFFI for Type-Safe Bindings**

I used Mozilla’s [UniFFI](https://github.com/mozilla/uniffi-rs) to generate Swift bindings. UniFFI handles FFI complexity while keeping type safety intact.

Original Rust function with UniFFI decorators:

```
#[uniffi::export]
pub fn create_collection(name: String) -> FfiResult<String> { /* ... */ }
```

Generated Swift function:

```
public func createCollection(name: String) throws -> String { /* ... */ }
```

This approach means, by using the ChromaSwift package,  Swift developers can call Chroma APIs with native type safety, while still benefiting from Rust’s performance.

------



## **Custom Rust -> Swift Wrappers**

There were a few areas where I had to implement some custom wrapping of Rust types to bridge betwen Swift and Rust:

### **Error Handling**

Custom UniFFI error wrapper:

```
#[derive(Debug, Error, uniffi::Error)]
pub enum ChromaError {
    #[error("{message}")]
    Generic { message: String },
}
```

### **Async Operations**

Rust async bridged via a global Tokio runtime:

```
let result = runtime.block_on(async {
    frontend.some_async_operation().await
})?;
```

------



## **Cross-Platform Compilation**

The build script compiles for Apple iOS and macOS targets:

```
RUST_TARGETS=(
  "aarch64-apple-darwin"      # macOS Apple Silicon
  "x86_64-apple-darwin"       # macOS Intel
  "aarch64-apple-ios"         # iOS Device
  "x86_64-apple-ios"          # iOS Simulator (Intel)
  "aarch64-apple-ios-sim"     # iOS Simulator (Apple Silicon)
)
```

Universal binaries are merged with `lipo` and shipped as an XCFramework for seamless distribution. This framework is hosted on the [GitHub releases page](https://github.com/chroma-core/chroma-swift/releases/), and is used by the SwiftPackage 

------



## **API Design**

The Swift package exposes two levels of APIs:

### **Direct FFI Functions**

```
try Chroma.initialize(allowReset: true)
let collectionId = try Chroma.createCollection(name: "docs")
try Chroma.addDocuments(
    collectionName: "docs",
    ids: ["doc1"],
    embeddings: [[0.1, 0.2, 0.3]],
    documents: ["Hello world"]
)
```

These are generated by `uniffi`, and call directly in to the cross-compiled `XCFramework`.



## **Local Embedding Models**

The package integrates [MLXEmbedders](https://github.com/ml-explore/mlx-swift) to support on-device embeddings. It ships with **8 models** (17MB to 1.3GB), offering trade-offs between speed and quality:

- **BGE Micro** (17MB, 384D)
- **MiniLM L6** (90MB, 384D) 
- **BGE Base** (440MB, 768D) — Higher quality
- **BGE Large** (1.3GB, 1024D) — Maximum quality



### **High-Level Embedder API**

```
let embedder = ChromaEmbedder(model: .miniLML6)
await embedder.loadModel()

let count = try await embedder.addDocuments(
    to: "docs",
    ids: ["doc1"],
    texts: ["Hello world"]
)
```

The Embedder API makes it easy to work with text — automatically generating embeddings that can then be stored and queried in ChromaSwift. 

Of course, you can always generate embeddings via an API call to a 3rd party service such as OpenAI or Cohere, get the embeddings back, and then store them in Chroma.



## **Storage & Sync Options**

Chroma Swift supports multiple storage patterns:

### **Ephemeral Storage**

```
try Chroma.initialize(allowReset: true)
```

Useful for in-memory prototyping.

### **Persistent Storage**

```
let path = FileManager.default.urls(
    for: .documentDirectory,
    in: .userDomainMask
).first!.appendingPathComponent("chroma_db").path

try Chroma.initializeWithPath(path: path, allowReset: false)
```

In persistent mode, Chroma stores your embeddings across user sessions via SQLite. You can configure it to be saved at whatever path is most appropriate. 



## Example Usage 

End-to-end example for initializing Chroma (in Ephemeral mode), generating embeddings, creating documents, and querying for them.

```
import Chroma

// Run from an async context, e.g. Task { try await runChromaExample() }
@MainActor
func runChromaExample() async throws {
    // 1) Initialize Chroma (ephemeral / in-memory)
    try Chroma.initialize(allowReset: true)

    // 2) Create a collection
    let collection = "docs"
    _ = try Chroma.createCollection(name: collection)

    // 3) Load an on-device embedder (MLX). Choose a model that fits your size/speed needs.
    let embedder = ChromaEmbedder(model: .miniLML6) // e.g. .bgeMicro, .miniLML6, .bgeBase, .bgeLarge
    await embedder.loadModel()

    // 4) Add some documents (embeddings are generated locally)
    let items: [(id: String, text: String)] = [
        ("doc1", "Swift is a modern language for building fast, safe apps on Apple platforms."),
        ("doc2", "Rust focuses on memory safety and performance with zero-cost abstractions."),
        ("doc3", "MLX enables efficient on-device inference on Apple Silicon.")
    ]

    _ = try await embedder.addDocuments(
        to: collection,
        ids: items.map(\.id),
        texts: items.map(\.text)
    )

    // 5) Query: embed the query text locally, then search 
    let queryText = "Which language emphasizes memory safety?"
    let queryEmbedding = try await embedder.embed(queryText)

    // You can also control nResults / include metadata depending on your API surface
    let results = try Chroma.queryCollection(
        collectionName: collection,
        queryEmbeddings: [queryEmbedding],
        nResults: 3
    )

    // 6) Inspect results (ids/documents/distances are typically grouped per-query)
    print("Top IDs:", results.ids.first ?? [])
    print("Top Documents:", results.documents?.first ?? [])
    print("Distances:", results.distances?.first ?? [])
}

// Example kick-off:
// Task { try await runChromaExample() }
```



## **Demo Applications**

The repository includes **four demo apps**:

- ***EphemeralChroma***: for quick experiments without persistence.
- ***PersistentChroma***: shows SQLite-backed storage across sessions.
- ***LocalEmbeddingsModelDemo***: demonstrates running MLX models on-device.
- ***ChromaCloudSync***: hybrid local + cloud workflow.



##  **Further Work**

Issues and PR's are welcome! Some things you could work on include:

* Adding test coverage 
* More documentation 
* More examples

------

ChromaSwift, like Chroma itself, is licensed under the the [Apache 2.0 License](https://github.com/chroma-core/chroma-swift/blob/master/LICENSE). Please feel free to file issues if you run into anything or open a pull-request. Looking forward to seeing what you make with it!

*Chroma Swift is open source and available today. Explore the* [*repository*](https://github.com/chroma-core/chroma-swift) *for installation details and example projects.*

------



