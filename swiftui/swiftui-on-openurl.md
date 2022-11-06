# onOpenURL handler does not have to be attached to the toplevel view

While coding support for opening URLs on my SwiftUI application, I was
struggling with the entire workflow necessary to pass information
collected at the top level of my application.  I had a toplevel Tab
bar, and then assorted NavigationViews, and as I started to do all the
necessary plumping to push things down, I tried just moving my
`onOpenUrl` handler next to the view that would handle it, and to my
surprise, this got the event handler working and I did not have to do
any of the additional work.

