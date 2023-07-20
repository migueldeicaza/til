While Swift offers some basic reflection capabilities on its API, there is a lot
that is missing.   

Today Doug pointed me to the `swift-inspect` tool that is part of the Swift.
This tool uses a library that is installed in the system `SwiftRemoteMirror`
which comes with a C-based API for reflecting a Swift process.

On MacOS, the library is installed directly in the system under
`/usr/lib/swift/libswiftRemoteMirror.dylib` but the header files and the
`module.modulemap` are part of Xcode and located at
`/Applications/Xcode.app/Contents/Developer/Toolchains/XcodeDefault.xctoolchain/usr/include/swift/SwiftRemoteMirror/`

Sadly, `swift-inspect` does not seem to be maintained in the `swift` build, but
it is possible to build it with a few steps:

* Create a `Sources/SwiftRemoteMirror directory` inside `swift-inspect`
* Copy the contents of the
  `/Applications/Xcode.app/Contents/Developer/Toolchains/XcodeDefault.xctoolchain/usr/include/swift/SwiftRemoteMirror/`
  into it
* Add `linkerSettings: [.linkedLibrary ("swiftRemoteMirror")]` to
  `Package.swift`
* Add `.systemLibrary(name: "SwiftRemoteMirror")`


And then `swift-build` should be able to build your version of `swift-inspect`



