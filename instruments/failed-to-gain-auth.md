When you get the error "Failed to gain authorization" to run Instruments on an
application, it is caused by the executable not being signed.

A quick hack from
[here](https://forums.developer.apple.com/forums/thread/681687) is to sign it
like this, create a file `debug.plist` that has this content:

```
<?xml version="1.0" encoding="UTF-8"?><!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "https://www.apple.com/DTDs/PropertyList-1.0.dtd"><plist version="1.0"><dict><key>com.apple.security.get-task-allow</key><true/></dict></plist>
```

Then sign the app like this:

```
$ codesign -s - -v -f --entitlements /path/to/debug.plist /path/to/your/executable
```