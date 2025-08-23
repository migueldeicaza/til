# ColorScheme changes when the app goes to the background

Just ran into a peculiar bug, when an app is being sent to the background on
iOS, the system will post a theme change notification with the other theme, and
then back.

So if your app is running in light mode, before your app is sent to the
background, the system will post a notification switching it to dark mode, and
then post immediately another notification to go light.

I reproduced the effect with a small sample, and then manually switched the
themes - so this is used to render the previews of your app in the right color
if the color scheme changes.

Sample:

```

import SwiftUI

struct ContentView: View {
    @SwiftUI.Environment(\.colorScheme) var colorScheme

    var body: some View {
        VStack {
            Image(systemName: "globe")
                .imageScale(.large)
                .foregroundStyle(.tint)
            Text("Hello, world!")
        }
        .onChange(of: colorScheme) { old, new in
            print ("Goint from \(old) to \(new)")
        }
        .padding()
    }
}

```
