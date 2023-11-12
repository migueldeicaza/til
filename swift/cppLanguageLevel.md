When doing interoperatbility with C++, you can pass a language-standards
to the C++ compiler by passing an argument to the `cxxLanguageStandard:`
parameter in the call to Package.

Like this:

```
// swift-tools-version: 5.9
// The swift-tools-version declares the minimum version of Swift required to build this package.

import PackageDescription

let package = Package(
    name: "Sample",
    products: [
        .library(
            name: "Godot",
            targets: ["Godot"])
    ],
    targets: [
        .target(
            name: "Godot",
            dependencies: ["CGodot"],
            path: "swift/Sources/Godot",
            cSettings: [.headerSearchPath("include")],
            swiftSettings: [.interoperabilityMode(.Cxx)]),
        .target(
            name: "CGodot",
            path: "swift/Sources/CGodot",
            cxxSettings: [
                .headerSearchPath("include/platform/macos"),
                .headerSearchPath("include"), 
                .unsafeFlags(
                    ["-Wno-c++11-extensions"])
            ]
        ),
        .testTarget(
            name: "GodotTests",
            dependencies: ["Godot"]),
    ],
    cxxLanguageStandard: .gnucxx17

)
```