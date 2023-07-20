Swift does not come with an XML deserializer/decoder, but there is a
neat open source project called
[XMLCoder](https://github.com/CoreOffice/XMLCoder) that provides this
capability.

One thing that I could not figure out from their documentation was how
to get data out of this XML:

```xml
<demo>
   <content title="Welcome">My Content</content>
</demo>
```

But their test suite revealed a neat trick.   When you define your codable, add a CodingKeys for `value`, like this:

```swift
struct Content: Codable, Equatable {
    @Attribute var title: String
    let value: String
    
    enum CodingKeys: String, CodingKey {
        case title
        case value = ""
    }
}
```

And this will deserialize the "My Content" into the `value` variable.