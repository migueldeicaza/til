I could not find out why I was not receiving a call to my `.onEnded` or
`.onChanged` methods in a sequential gesture in SwiftUI, and the reason is that
the `.onEnded` only happens when the gesture is successful, and the `.onChanged`
is only called when the gesture is in progress.

When using the @GestureState property wrapper, this will take care of resetting
the value to the initial value when the gesture ends.  So to track when a
gesture has completed, you can monitor the value of the @GestureState property
using the `.onChange` modifier, and deal with it there.

More information on sequential gestures can be found in [Composing SwiftUI Gestures](https://developer.apple.com/documentation/swiftui/composing-swiftui-gestures)