I needed to turn some markdown text into an NSAttributedString, and
figured that someone must have written something like this.  SwiftUI's
own `Text()` view supports a limited set of Markdown operations.

Googling did not help me much, as the answers that I got were projects
that were not very actively maintained, and lacked some features I
needed.  It is clearly a space that has evolved in the last few years,
and while some projects have been flagged as archived on GitHub, I
could not find what folks were using these days.

But eventually found to
[Markdownsaur](https://github.com//Markdownosaur) by Christian Selig
which is itself powered by [Apple's Swift
Markdown](https://github.com/apple/swift-markdown) library.  While
Apple's library is great, it is a lower-level library that produces
ASTs for Markdown, and you would need to roll out the actual rendering
- luckily Christian's library solves this.

