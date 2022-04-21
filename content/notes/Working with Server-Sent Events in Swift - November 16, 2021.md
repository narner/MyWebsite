---
title: "Working with Server-Sent Events in Swift"
date: "2021-11-16"

---



In most mobile applications that work with networked data, standard HTTP GET requests are used to fetch the data from the server that you want to display in your app's UI.

However, some backend servers may make use of a different pattern for data transfer between themselves and a client than the common REST architecture - that of[ Server Sent Events (SSE's)](https://en.wikipedia.org/wiki/Server-sent_events).

In this scenario, a server broadcasts any updated data, and the client application listens for these changes to come in over the HTTP connection. As long as that connection remains open, the client will be aware of any changes that have occurred server-side.

In this post, I'll walk through how to implement an iOS app to connect to an event source using the[ EventSource Library](https://github.com/inaka/EventSource).

&nbsp;

## Setting up the EventSource and Receiving Data

In the function below, we'll first setup our EventSource with the URL of the server that we're connecting to:

```swift
func setupEventSource(channelURLString: String) {
	let serverURL = URL(string: channelURLString)!
	eventSource = EventSource(url: serverURL, headers: ["Authorization" : cookie!.value])
	eventSource?.connect()
}
```

Inside this function, we'll register our callbacks for interacting with the EventSource. The first one, onComplete, is called if the EventSource is closed for whatever reason. This could happen if there's an error at the network layer, loss of connectivity, or the server itself requests a disconnection.

```swift
eventSource?.onComplete({ [self] (statusCode, reconnect, error) in
	eventSource?.connect(lastEventId: currId);
})
```

The next callback is called once our EventSource has successfully connected to the upstream server, and is ready to receive data from it:

```swift
eventSource?.onOpen {
	print("Event sourced opened!")
}
```

Finally, we register a callback that gets called whenever we have received a message over the EventSource. This gives us back an id, an event, and data from our server.

```swift
eventSource?.onMessage({ [self] (id, event, data) in
	guard let id = id else {
		return
	}
            
currId = id;
```

&nbsp;

## Receiving and Processing Data from the Server

Now that we’ve set-up our EventSource receiver and connected to our remote server, we’re ready to process any data that our server sends us. Here, we set the value of `id` to a global variable, `currentID`:

```swift
var currId = ""
```

We'll use this to keep track of whatever the most recent event ID from our event source was. This value gets used in the `onComplete` callback above to reconnect with the server in the event that our app gets disconnected from it for some reason.

The data we get back is represented as a `String`: 

```swift
guard let dataString = data else {
	return
}
```

If you're receiving JSON data, and want to convert it to a Dictionary, you can do so with a function like this:

```swift
func convertToDictionary(text: String) -> [String: Any]? {
	if let data = text.data(using: .utf8) {
		do {
			return try JSONSerialization.jsonObject(with: data, options: []) as? [String: Any]
		} catch {
			print(error.localizedDescription)
		}
	}
	return nil
}
```

Here's the full function that we use to set-up our EventSource and register the three callbacks within it: `onComplete`, `onOpen`, and `onMessage`.

```swift
func setupEventSource(channelURLString: String) {
        
	let serverURL = URL(string: channelURLString)!
	eventSource = EventSource(url: serverURL, headers: ["Authorization" : cookie!.value])
	eventSource?.connect()
        
	eventSource?.onComplete({ [self] (statusCode, reconnect, error) in
		eventSource?.connect(lastEventId: currId);
	})
        
	eventSource?.onOpen {
		print("Event sourced opened!")
	}
        
	eventSource?.onMessage({ [self] (id, event, data) in
		guard let id = id else {
			return
		}
            
		currId = id;
            
		guard let dataString = data else {
			return
		}
            
		guard let dict = dataString.convertToDictionary(text: dataString) else {
			return
		}
	})
}
```

Now, we're all set up to connect to our EventSource and receive data from it in our app. 

&nbsp;

## Reconnection

If someone dismisses our app to the background, we have no guarantee for how long our EventSource will stay open. In order to re-establish connection with our server when the app is opened again, we can set-up a notification listener in our ViewController to detect when the app has moved back into the foreground:

```swift
NotificationCenter.default.addObserver(self, selector:#selector(appMovedToForeground), name: UIApplication.willEnterForegroundNotification, object: nil)
```

When the ViewController receives the notification that the app has moved back into the foreground, we can then call the `appMovedToForeground` function, which re-establishes the connection with our server if the EventSource is closed:

```swift
@objc func appMovedToForeground() {
	if eventSource?.readyState == .closed {
		resetEventSource { [self] in
		}
	}
}
```

Here's the function for resetting the EventSource - we make sure to manually disconnect before connecting again, just to make sure that we don't ever enter a state where we're accidentally trying to connect to the EventSource if we're already connected.

```swift
func resetEventSource(completion: @escaping () -> Void) {
	eventSource?.disconnect()
	eventSource?.connect()
	completion()
}
```



## Final Thoughts

Working with Server-Side Events in an iOS app is not probably something you’ll encounter very often, but if you do; remember that the pattern you’re used to working with has been flipped around - you’re listening for updates from the server, not *polling* forupdates from the server. 
