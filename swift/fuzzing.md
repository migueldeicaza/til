# Fuzzing in Swift

I love fuzzing my libraries, but the support for Fuzzing in Swift is
not part of the standard toolchain, it is necessary to install a
nightly snapshot that includes this support to run it.


## Your code

Put together a new project that references the library that you want
to fuzz in your Package.swift, it can be a single file that contains this entry point:

```

@_cdecl("LLVMFuzzerTestOneInput")
public func fuzzMe(data: UnsafePointer<UInt8>, length: CInt) -> CInt {
    let buffer = UnsafeBufferPointer(start: data, count: Int (length))
    let array = Array (buffer)
    
    call_your_function_to_fuzz (data: array)
}
```

## Tooling

On macOS, the easiest way to do it, is to download [a release
snapshot](https://www.swift.org/download/] and install that on your
Mac.   Then, you need to do a few things.

* Extract the Toolchain version, you can do it like this, in my case,
  I installed the 5.7.3 release version:

```
$ plutil -extract CFBundleIdentifier raw /Library/Developer/Toolchains/swift-5.7.3-RELEASE.xctoolchain/Info.plist
```

I put this in the `TOOLCHAINS` environment variable.

* You will use the resulting code to invoke the swift compiler:

```
$ xcrun --toolchain $TOOLCHAINS swift --version
```

Now, to fuzz, you want to compile with a couple of additional command line arguments:

```
$ xcrun --toolchain $TOOLCHAINS swift build -Xswiftc "-sanitize=fuzzer" -Xswiftc "-parse-as-library"
```

Then run your fuzzed binary, you can tune some of these parameters, but this is what I use:

```
./.build/debug/YOUR_PROGRAM ../Corpus -rss_limit_mb=40480 -jobs=12
```

