On and off I run into issues with my own packages, or third party packages that
use dynamic libraries, and it manifests with a startup crash like this on device:

```
dyld[4569]: Library not loaded: @rpath/SwiftGodot.framework/SwiftGodot
  Referenced from: <890EC7B9-FAAC-3D44-BC20-1F9787E2FB75> /private/var/containers/Bundle/Application/8393419D-A564-4929-8BB8-17C0CD8AA1B5/Xogot.app/Xogot
  Reason: tried: '/Users/miguel/Library/Developer/Xcode/DerivedData/Xogot-bgbnzewtbyvphkcakgjczczexopk/Build/Products/Debug-iphoneos/PackageFrameworks/SwiftGodot.framework/SwiftGodot' (no such file), '/private/preboot/Cryptexes/OS/Users/miguel/Library/Developer/Xcode/DerivedData/Xogot-bgbnzewtbyvphkcakgjczczexopk/Build/Products/Debug-iphoneos/PackageFrameworks/SwiftGodot.framework/SwiftGodot' (no such file), '/private/var/containers/Bundle/Application/8393419D-A564-4929-8BB8-17C0CD8AA1B5/Xogot.app/Frameworks/SwiftGodot.framework/SwiftGodot' (no such file), '/Users/miguel/Library/Developer/Xcode/DerivedData/Xogot-bgbnzewtbyvphkcakgjczczexopk/Build/Products/Debug-iphoneos/PackageFrameworks/SwiftGodot.framework/SwiftGodot' (no such file), '/private/preboot/Cryptexes/OS/Users/miguel/Library/Developer/Xcode/DerivedData/Xogot-bgbnzewtbyvphkcakgjczczexopk/Build/Products/Debug-iphoneos/PackageFrameworks/SwiftGodot.framework/SwiftGodot' (no such file), '/private/var/containers/Bundle/Application/8393419D-A564-4929-8BB8-17C0CD8AA1B5/Xogot.app/Frameworks/SwiftGodot.framework/SwiftGodot' (no such file)
```

The solution is to set the dynamic library option in the Target section in Xcode
to "Embed and Sign":

1. "Project Navigator"
2. Select your project from the top of the tree.
3. Under "Targets", select your project
4. Go to "General"
5. Go to "Frameworks, Libraries and Embedded Content"
6. Select your library
7. Make sure it is set to "Embed & Sign"
