I have been struggling with the `.popup` modifier when I had to provide a size
for a popup that contained a VStack with a List, it would create an odd layout
that would push part of the VStack out and it would not be possible to see.

It seems like a solution is to set a "maxHeight" rather than a "height"
parameters.   I guess this does not force the "height" when it can not meet the
requirement, and you complement that with a call to "idealHeight".