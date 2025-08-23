I needed a custom version of the `.alert` modifier or the `.confirmationDialog`,
but one that would let me provide additional controls or views inside that view.
So I resorted to a `.sheet` for it.

Emulating the button layout is not difficult, it was like this:

```
public var body: some View {
        VStack {
            VStack {
                Text(title)
                    .font(.headline)
                    .padding(.horizontal, 80)
                content()
            }
            .padding()

            VStack(spacing: 0) {
                Divider()
                HStack {
                    Button("Cancel", role: .cancel) {
                        dismiss()
                        callback(false)
                    }
                    .frame(maxWidth: .infinity)
                    Divider()

                    Button(acceptTitle) {
                        callback(true)
                        dismiss()
                    }
                    .padding()
                    .frame(maxWidth: .infinity)
                }

            }
        }
        .presentationSizing(.fitted.sticky(horizontal: false, vertical: true))
    }
```

Then I found out a few things: first, my buttons were not like the SwiftUI ones
that could allow the entire region of the button to be tapped, not text.   Ouch.

The other one is that the platform version was using some kind of translucent
material, not just the default system background that you get from `.sheet`, and
then I noticed a nice effect when you press the button that gives some feedback
that the button is being pressed.

Anyways, long story short, I am sharing the code snippet that achieves all of
those goals here:

```swift

/// A dialog that is suitable to add your custom content to it, but follow the
/// platform idioms.
///
/// Present this with a `.sheet` modifier
///
/// Would love to remove the buttons and make those generic, like `alert` does
/// 
public struct InteractionDialog<Content: View>: View {
    @Environment(\.dismiss) var dismiss
    @State var name = ""
    let callback: (_ result: Int?) -> ()
    let content: () -> Content
    let title: String
    let buttons: [ButtonKind]

    public enum ButtonKind {
        /// This creates a cancel button, and it gets assigned the role `cancel`
        case cancel
        /// An action button contains the text, and the number to send back as a result
        case action(LocalizedStringKey,Int)
        /// Use this to create a button with the role `destructive`
        case destructive(LocalizedStringKey, Int)
    }
    /// Creates a new Interaction Dialog
    /// - Parameters:
    ///   - title: The title to show
    ///   - buttons: optional, but contains a description of the buttons you want, the callback
    ///     is invoked with `nil` if the cancel button is called, otherwise an integer associated with the label.
    ///     if you do not specify this value this defaults to `[.cancel, .action("Ok", 0)]`, so you will
    ///     get a nil for cancellation and a 0 for the user pressing ok.
    ///   - content: Your view contents to display
    ///   - callback: This method is invoked, with a true value if the user pressed ok, and false if the user pressed cancel
    public init(title: String, buttons: [ButtonKind] = [.cancel, .action("Ok", 0)], @ViewBuilder content: @escaping () -> Content, callback: @escaping (_ result: Int?) -> ()) {
        self.title = title
        self.content = content
        self.callback = callback
        self.buttons = buttons
    }

    /// This style is applied for two reasons, one is to make the background darker when the button is pressed
    /// which imitates the Alert button style.   The other is to make the whole region that we force the button to use
    /// to be tappable - not only the text region.
    struct AlertLikeStyle: ButtonStyle {
        let isEnabled: Bool

        @ViewBuilder
        func makeBody(configuration: Configuration) -> some View {
            let pressedColor = Color(uiColor: .tertiaryLabel).opacity(0.3)

            configuration.label
                .foregroundStyle(configuration.role == .destructive ? .red : .accentColor)
                .background(configuration.isPressed
                            ? pressedColor
                            // This very transparent color is needed to get taps in the
                            // whole button region, otherwise we only catch it on the foreground
                            // text
                            : Color(uiColor: .systemBackground).opacity (0.001))
        }
    }

    @ViewBuilder
    func makeButton(_ button: ButtonKind) -> some View {
        Group {
            switch button {
            case .action(let text, let code):
                Button(action: {
                    dismiss()
                    callback(code)
                }) {
                    Text(text)
                        .padding()
                        .frame(maxWidth: .infinity, maxHeight: .infinity)
                }
            case .cancel:
                Button(role: .cancel, action: {
                    dismiss()
                    callback(nil)
                }) {
                    Text("Cancel")
                        .padding()
                        .frame(maxWidth: .infinity, maxHeight: .infinity)
                }
            case .destructive(let text, let code):
                Button(role: .destructive, action: {
                    dismiss()
                    callback(code)
                }) {
                    Text(text)
                        .padding()
                        .frame(maxWidth: .infinity, maxHeight: .infinity)
                }
            }
        }
        .buttonStyle(AlertLikeStyle(isEnabled: true))
    }

    public var body: some View {
        VStack {
            VStack {
                Text(title)
                    .font(.headline)
                    .padding(.horizontal, 80)
                content()
            }
            .padding()

            VStack(spacing: 0) {
                Divider()
                HStack(spacing: 0) {
                    if buttons.count > 1 {
                        ForEach(Array(buttons.dropLast().enumerated()), id: \.offset) { off, v in
                            makeButton(v)
                            Divider()
                        }
                    }
                    if let last = buttons.last {
                        makeButton(last)
                    }
                }

            }
        }
        .presentationBackground(.regularMaterial)
        .presentationSizing(.fitted.sticky(horizontal: false, vertical: true))
    }
}

#if DEBUG
#Preview {
    @Previewable @State var showAlert = false
    @Previewable @State var show = true
    ZStack {
        Color.red

        VStack {
            Button("Show Alert") { showAlert = true }
                .buttonStyle(.borderedProminent)
            Button("Show Dialog") { show = true }
                .buttonStyle(.borderedProminent)

            Text("Anchor")
                .sheet(isPresented: $show) {
                    InteractionDialog(title: "Renaming animation", buttons: [.cancel, .action("Skip", 1), .destructive("Overwrite", 2)]) {
                        Text("Here I am")
                        TextField("Foo", text: .constant(""))
                        Toggle(isOn: .constant(true)) {
                            Text("Demo")
                        }
                    } callback: { result in }
                        .presentationBackground(.regularMaterial)
                }
                .alert("Demo for Tap Region", isPresented: $showAlert) {
                    Button("OK") { showAlert = false }
                    Button("Cancel") { showAlert = false }
                }

        }
    }
}
#endif

```