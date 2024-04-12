You can add a ToolbarItem with a placement of `.keyboard` and the view that you
pass to it will be show as a keyboard accessory when the input shows up:

```swift
.toolbar {
    ToolbarItem(placement: .keyboard) { Text ("kbd") }
}
```