In SwiftUI, you can use an @Environment value to track the display scale, what
you previously used `UIScreen.main.displayScale`, but unlike the UIScreen one,
this works automatically with multiple displays, using [`@Environment(\.displayScale) var displayScale`](https://developer.apple.com/documentation/swiftui/environmentvalues/displayscale)

There are various other [useful
variables](https://developer.apple.com/documentation/swiftui/layout-adjustments#reacting-to-interface-characteristics)
to respond to interface characteristics available.

