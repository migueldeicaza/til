In the past I resorted to assorted hacks to keep scrolling views tracking the
bottom of the screen (like in my ChatGPT client), now SwiftUI offers a feature
to do this:

```
ScrollView {
	List {
		LotsOfItems()
	}
}
.defaultScrollAnchor(.bottom)
```

