Today I found myself wanting to get a description to a method in
Swift, but not have it associated with an existing instance, so I
could call this method with any instances I might have wanted.

This is often call function currying, and I had a vague recollection
that this was one of the features of Swift when it was first launched,
but had never had to use it.

In C# this would be accomplished by using Reflection and getting the
MethodInfo and invoke it, something like:

``
// Reflect the class, then get the MethodInfo, then call this:

callFunction (MethodInfo info, Object instance, Object [] args) {
    info.Invoke (instance, args)
}
```

It turns out that in Swift this is baked directly into the language.

Imagine the following class, and your desire to have a function
description that can call the method not with a particular instance,
but any arbitrary instance:

```swift
class Demo {
      var title: String
      init (_ title: String) { self.title = title }
      func add (_ x: Int, y: Int) -> String {
          return "The sum of \(title) \(x+y)"
      }
}

let cookies = Demo ("Cookies")
let pets = Demo ("Pets")

let adder: (Demo) -> (_: Int, _: Int) -> String

adder = Demo.add

print (adder (cookies) (1, 2))
// prints "The sum of Cookies is 3
print (adder (pets) (4, 2))
// prints "The sum of Pets is 6
```

