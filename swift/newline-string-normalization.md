# Swift newline string normalization

When Swift imports a string, it will turn the sequence carriage
return, newline (\r\n) into a newline (\n).

```swift
print ("a\nb".count)	// prints 3
print ("a\r\nb".count) 	// prints 3
```

Even if you iterate over those values, you can see the newline:

```swift
for c in "a\nb" { print (c.asciiValue!) }
// prints
// 97
// 10
// 98
```

```swift
for c in "a\r\nb" { print (c.asciiValue!) }
// prints
// 97
// 10
// 98
```

So far, so good.  But if you try to find the newline in this string,
it will not work:

```swift

if "a\nb".firstIndex (of: "\n") != nil { print ("found") } else { print ("not found")  }
// prints:
// found
if "a\r\nb".firstIndex (of: "\n") != nil { print ("found") } else { print ("not found")  }
// prints:
// not found
```

This is problematic if you wanted to split the input string using
newlines, as it does not consider this string as containing new lines
at all!

```swift
"a\nb".split (separator: "\n").count
// returns 2
"a\r\nb".split (separator: "\n").count
// returns 1
```

You could use "\r\n" as your separator to get the right result, but
you would need to know that this is what you have to use to split your
string:

```swift
"a\r\nb".split (separator: "\r\n").count
// returns 2
```

An alterantive is to use this:

```swift
"a\nb".split(omittingEmptySubsequences: false, whereSeparator: { $0.isNewline }).count
// returns 2
"a\r\nb".split(omittingEmptySubsequences: false, whereSeparator: { $0.isNewline }).count 
// returns 2
```

