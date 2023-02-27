# Observing a Nullable State

I found myself wanting to keep track of various properties that would be
optionally set.   This works great as long as you are using `@State var
variable: Type? = nil`, but you end up with messy code if you have too many of
those.

And I wanted to observe changes to it.

In an ideal world, I wanted to be able to have a nullable observable, like this:

```swift
struct MyView: View {
	@ObservedObject var myObject: MyObject? = nil
}
```

But it is not possible to have a nullable ObservedObject.

The solution to this problem is to observe an object that can have a nullable
state, like this:

```swift

class MyObject: ObservableObject {
    struct MyState {
	var a: Int
	var b: String
    }
    @Published var myState: MyState? = nil
}
```

And you can then observe changes to `myState`, both from nil to having a value,
as well as any changes to the value itself.