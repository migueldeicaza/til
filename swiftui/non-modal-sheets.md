# Non-modal sheets

On iOS, it is possible to display a sheet, but allow the user to interact with
the background, this requires two things: (a) a presentation detent that allows
the sheet to take less space (I use `[.medium, .large]`) and the magic call that
is very hard to find online, on their docs, or in any blog posts:
`/presentationBackgroundInteraction(upThrough: .medium)`

For example:

```
        .sheet(item: $showSheet, onDismiss: {
            unselectBottomTab()
        }, content: { t in
            Group {
                Text("My sheet \(t)")
            }
            .background(Color(uiColor: .secondarySystemBackground))
            .presentationDetents([.medium, .large], selection: $selectedDetent)
            .presentationDragIndicator(.visible)
            .presentationBackgroundInteraction(.enabled(upThrough: .large))
        })
    ```