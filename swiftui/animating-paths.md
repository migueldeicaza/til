Images rendered in SwiftUI using `Path` do not get automatically animated if any
parameters they used changed.   To do this, it is necessary to create a
dedicated `Shape` structure, and implement the `path(in:)` method.   This method
will contain your usual drawing code.

To actually animate the path, you must create a variable `var animatableData` of
containing the data that will be animated, this property must have a setter and
a getter.

If more than one property is to be animated, use `AnimatablePair` which is a
tuple with the values that will be animated.

Also, I learned that if you only animate one value, but two change, the
animation is funky, so even if they are related, make sure that all the values
that are changed in your `withAnimation` are tracked there.