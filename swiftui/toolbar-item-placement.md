For La Terminal on iOS and iPad I have been placing my buttons for
assorted actions manually on the left or right side, using either
`.navigationBarTrailing` or `.navigationBarLeading` and using some
judgement on where things should go based on the interface.

With the upcoming Mac port, these verbs do not exist, and while it is
possible to put them elsewere, it is a good opportunity to replace
these with the semantic placement options which will get the right
location for each platform.

Apple documentation shows where things go, but did not have a
convenient chart for it, so here is my cheat sheet for it:

# Semantic Placement

|               |iPhone/iPad/tvOS|Mac/MacCatalyst|
|---------------|----------------|---------------|
|`.principal`   |Center of the navigation bar.This item takes precedent over a title specified through View/navigationTitle.|Center of the toolbar.|
|`.navigation`  |Leading edge of the navigation bar. If a system navigation item such as a back button is present in a compact width, it instead appears in the primaryAction placement.| Leading edge of the toolbar ahead of the inline title if that is present in the toolbar.|
|`.status`      |Center of the bottom toolbar (no tvOS)|Center of the Toolbar|


|               |iPhone/iPad/tvOS                    |Mac/MacCatalyst                                              |watchOS|
|---------------|------------------------------------|-------------------------------------------------------------|-------|
|`.primary`     |Trailing edge of the navigation bar.|Leading edge of the toolbar.                                 |Beneath the navigation bar.|
|`.secondary`   |Not required|
|`.confirmation`|Same location as `.primary`         |Trailing edge in the trailing-most position of the sheet     |Trailing edge of the navigation bar.|
|`.cancellation`|Leading edge of the navigation bar. |Trailing edge of the sheet, before confirmationAction items. |Leading edge of the navigation bar.|
|`.destructive` |Trailing edge of the navigation bar.|Leading edge of the sheet + special appearance               |Trailing edge of the navigation bar.|
		