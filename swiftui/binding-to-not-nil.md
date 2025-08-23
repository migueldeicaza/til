Found that there is a [cute
constructor](https://developer.apple.com/documentation/swiftui/binding/init(_:)-6krsi)
that you can use to bind to a local value that is the non-optional version,
useful when you have a binding to an optional, but need to pass this to a place
that does not take an optional:

```swift
struct ContentView: View {
    @Binding private var optText: String?
    
    var body: some View {
	if let text = optText {
	    TextField("Enter text", text: Binding(text))
	}
    }
}
```