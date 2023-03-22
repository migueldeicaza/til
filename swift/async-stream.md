I love AsyncStream.  A good post is
[here](https://www.donnywals.com/understanding-swift-concurrencys-asyncstream)
and Apple's official documentation is [here](https://developer.apple.com/documentation/swift/asyncstream)

In particular, I wanted to wrap the async results I was getting from
an existing library, but I wanted to do some additional processing
before surfacing it, so I wrote something like this:

```
return AsyncThrowingStream (bufferingPolicy: .unbounded) { continuation in
    Task {
        for try await line in bytes.lines {
	    continuation.yield(decode (line))
        }
        continuation.finish()
    }
}
```

And then you can use the above in a nice `for try await` loop yourself