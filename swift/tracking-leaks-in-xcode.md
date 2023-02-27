# Tracking Leaks in Xcode

Years ago, I used to use Instruments as my tool to find leaks, but
nowadays, Xcode includes a convenient built-in leak detector.  To
activate it, you tap the "Debug Memory Graph" on your running
application:

![Debug Memory Graph](https://user-images.githubusercontent.com/36863/217953188-9b39cce1-c4ef-4272-88f5-6cabb6bc80a1.png)

And while this is super-useful on its own, when it comes to tracking
down a memory leak that involves swift closures and other ugly beasts,
what you want to do is enable Malloc logging, you do this from the
project scheme:

![Malloc Stack Logging](https://user-images.githubusercontent.com/36863/217953769-b5d5a55e-d8e0-44c8-9e3e-c033b999118a.png)

Once you do that, when you find the culrpit for your woes, you can
then see the stack trace on the right:

![Stack Trace](https://user-images.githubusercontent.com/36863/217954297-1a11e099-041f-4907-b8c0-f892a503804a.png)

