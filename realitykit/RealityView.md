Would it kill Apple to document the parameters to [`RealityView`](https://developer.apple.com/documentation/realitykit/realityview)?

I have been looking for when the `update` closure is called, but it is nowhere
to be found on the actual constructor documentation.   It is glossed over on the
summary, but the sole confirmation comes from the video "Developer Your First
Immersive App" where they state that the `update` closure is called in response
to SwiftUI view changes - this is key, it has no correlation to other update
methods in RealityKit and their systems.   This is a SwiftUI-level state change,
not anything else.