AppIntent can specify input parameters with the ParameterSummary, like this:

```swift
    static var parameterSummary: some ParameterSummary {
        Summary("List files at \(\.$paths) on \(\.$server).")
    }
```

Which works great for providing two bits of input, but if you want to
have additional parameters that the user can specify, there is a
not-so-obvious, and as usual, poorly documented way of describing additional
parameters, which result in a disclosure indicator in Shortcuts:

```swift
    static var parameterSummary: some ParameterSummary {
        Summary("List files at \(\.$paths) on \(\.$server).") {
            \.$reverse
            \.$sortOrder
        }
    }
```

Where `reverse` and `sortOrder` are a couple of additional parameters in the AppIntent