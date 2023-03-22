# Streaming HTTP requests

I needed a way of getting the results coming back from an HTTP request
as they came, not wait until they were completely delivered.   It was
surprisingly hard to find an answer to this question.   The option
for `URLSession` configuring a delegate and implement `func
urlSession(_ session: URLSession, dataTask: URLSessionDataTask,
didReceive data: Data)` just did not work for me, despite a couple of
hours of beating it, and I did not want to bring Alamofire as a
dependency for a quick hack.

Eventually, I found that once you build your `URLRequest` you can use
this new async feature:

```
guard let (bytes, response) = try? await session.bytes(for: request, delegate: self) else {
   return nil
}
```

The resulting `bytes` is of type `URLSession.AsyncBytes` and is one of
those neat Swift collections that work with the for loop and async,
and comes with assorted helper method, so you can write code like
this:

```
for try await byte in bytes {
   print (byte)
}
```

But even better, you can process the incoming data in lines, unicode
scalars or Characters, like this:

```
for try await line in bytes {
   print (line)
}
```
