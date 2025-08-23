# Allowing more than one kind of object to be dropped on a View

I thought that allowing more than one object to be dropped on a view would be a
simple matter of adding multiple `.drop` or `.dropDestination` modifiers to a
view, but it turns out that only the first modifier is used.

So this does not work:
```

struct ContentView: View {
	@State var url = URL(filePath: "/tmp/demo")
	@State var text = "Hi"

	var body: some View {
	VStack {
		HStack {
		Text("Text Source")
		}
		.frame(minWidth: 100, maxHeight: 100)
		.background{Color.blue}
		.draggable(text)

		HStack {
		Text("URL Source")
		}
		.frame(minWidth: 100, maxHeight: 100)
		.background{Color.blue}
		.draggable(url)

		VStack {
		Image(systemName: "globe")
			.imageScale(.large)
			.foregroundStyle(.tint)
		Text("Drop Items here")
		}
		.frame(minWidth: 100, maxWidth: 100)
		.padding()
		.background(Color.red)
		.onDrop(of: [.url], isTargeted: nil) { a, b in
		print("yes")
		return true
		}
		.onDrop(of: [.text], isTargeted: nil) { a, b in
		print("yes")
		return true
		}
	}
	}
	}
```

Because only the `[.url]` or the `[.text]` will be used, but not both.

The solution is to create a custom Transferrable that can support both payloads,
like this:

```
enum DropItem: Codable, Transferable {
	case none
	case text(String)
	case url(URL)

	static var transferRepresentation: some TransferRepresentation {
	ProxyRepresentation { DropItem.text($0) }
	ProxyRepresentation { DropItem.url($0) }
	}

	var text: String? {
	switch self {
		case .text(let str): return str
		default: return nil
	}
	}

	var url: URL? {
	switch self {
		case.url(let url): return url
		default: return nil
	}
	}
	}
```

And then use this in the `.dropDestination` method:

```
	myView
		.dropDestination(for: DropItem.self) { a, b in
		return true
		}
```

