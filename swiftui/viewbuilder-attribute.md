# Using @ViewBuilder attribute

I had this little piece of code:

```
struct ContentView: View {
    @State var show = false

    var body: some View {
	if show {
		Text("Hello, World!")
	} else {
		Button(action: {}) {
			Text ("Ok")
		}
	}
    }
}
```

This code compiles and works, but if you try to move this code into a function
that returns `some View`, like this:

```
func makeBody() -> some View {
	if show {
		Text("Hello, World!")
	} else {
		Button(action: {}) {
			Text ("Ok")
		}
	}
}
```

The Swift compiler complains with the following error:

```
Branches have mismatching types 'some View'
```

The solution is to use the `@ViewBuilder` attribute in the function, like this:

```
@ViewBuilder
func makeBody() -> some View {
	if show {
		Text("Hello, World!")
	} else {
		Button(action: {}) {
			Text ("Ok")
		}
	}
}
```
