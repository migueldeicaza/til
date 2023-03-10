I was enjoying myself using the SwiftUI Map View, which is quite cute,
but I quickly found out that it has a number of limitations, and I had
to do too much work that was already handled by MapKit.   In addition,
I was updating the locations every other second, and it created an
annoying flicker effect.

Extensive googling and chatgpting did not produce any results for how
to avoid the blinking.

Reading the online commentary indicates that this is not a very
actively developed view in the SwiftUI world, and that it might have
been built mostly for users of watchOS, and not for the Map
aficionado crowd.

So I put together a [MKMapView wrapper for
SwiftUI](https://github.com/migueldeicaza/MapWrapper) that handles
this.