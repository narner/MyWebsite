---
title: "TiktokenSwift"
date: "2025-08-14"
---



Today I open-sourced TiktokenSwift - A Native Swift Wrapper of OpenAI's Tiktoken Tokenizer. 

TiktokenSwift is an open-source Swift package that brings OpenAI's high-performance tiktoken tokenizer natively to Apple platforms. By providing a lightweight FFI bridge to the official tiktoken library, it enables Swift developers to integrate the same tokenization used by GPT-3.5, GPT-4, GPT-4o, and other OpenAI models directly into iOS and macOS applications.

1) You can check out the GitHub for the package here: https://github.com/narner/TiktokenSwift

2) My fork of OpenAI’s Tiktoken, with the the build scripts for generating the Swift bindings and package, is here: https://github.com/narner/tiktoken

3) I opened up a PR for OpenAI’s Tiktoken repo with these scripts here: https://github.com/openai/tiktoken/pull/431



###  **Key Features:**

- Full compatibility with OpenAI's standard encodings (cl100k_base, r50k_base, p50k_base, o200k_base, o200k_harmony)
- Native Swift API with async/await support
- Maintains the performance and accuracy of the original Python implementation
- Support for special tokens and detailed encoding information
- Cross-platform support with universal binaries for ARM64 and x86_64 architectures

 

### **Supported Encodings:**

TiktokenSwift supports all major OpenAI encodings:

- **gpt2** - The original GPT-2 encoding
  2. **r50k_base** - Used by GPT-2 and legacy models
  3. **p50k_base** - Used by Codex models
  4. **p50k_edit** - A variant of p50k_base with FIM (Fill-in-the-Middle) tokens
  5. **cl100k_base** - Used by GPT-3.5-turbo and GPT-4 models
  6. **o200k_base** - Used by GPT-4o and o3-mini models
  7. **o200k_harmony** - A variant of o200k_base with additional special tokens for structured outputs



### **Technical Implementation:**

The library leverages Swift's Foreign Function Interface to bridge Rust-based tiktoken functionality, providing a pure Swift API that feels natural to iOS and macOS developers. It includes comprehensive support for encoding and decoding operations, special token handling, and detailed tokenization metadata.

The included SwiftUI example application provides an interactive tokenization playground  where developers can experiment with different encodings, visualize token boundaries, and  understand how text is segmented by OpenAI's models—serving as both a practical development tool and an educational resource for understanding tokenization behavior



### **Use Cases:**

TiktokenSwift is ideal for developers building AI-powered applications that need accurate token counting for API cost estimation, context window management, or text preprocessing for OpenAI models. 



### **Demo App:**

The package includes an iOS SwiftUI app that allows you to switch between the various encoders provided in the Package, encode, and decode input text. 



![Titkoken Swift Demo](/blog_assets/2025/TiktokenSwiftDemo.gif)
