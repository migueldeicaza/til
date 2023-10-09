One of my C++ classes was being surfaced as an OpaquePointer to
swift.   The solution is to include the header <swift/briding>
into your source code, and then at the end of your class, put the
`SWIFT_UNSAFE_REFERENCE` annotation on it.

The file is located inside the Xcode.app bundle in the following path:
`Contents/Developer/Toolchains/XcodeDefault.xctoolchain/usr/include/swift/bridging`

