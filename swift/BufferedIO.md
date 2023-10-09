Swift's foundation classes do not really include an API for buffered IO, like
say stdio has.   Instead it exposes a low-level file-handle like API, which is
fine, but provides no buffering, so it is very easy to end up in a situation
where your process is dominated by small writes.

I found [this
answer](https://stackoverflow.com/questions/74347486/best-way-to-write-large-text-file-incrementally-in-swift/74349466#74349466)
by [Rob Napier](https://stackoverflow.com/users/97337/rob-napier) on
StackOverflow very useful.

I wrote a small reusable wrapper that implements the idiom in a reusable fashion:

```swift
public func bufferedWriter (on handle: FileHandle, bufferSize: Int = 16*1024, body: (_ : ((String)throws->Void))throws->Void) throws {
    var buffer: [UInt8] = []
    buffer.reserveCapacity(bufferSize)

    
    func writeFunc (_ s: String) throws {
        buffer.append (contentsOf: s.utf8)
        if buffer.count >= bufferSize {
            try handle.write (contentsOf: buffer)
            buffer.removeAll(keepingCapacity: true)
        }
    }
    try body (writeFunc)
    
    // Write the final buffer
    try handle.write(contentsOf: buffer)
}
```
