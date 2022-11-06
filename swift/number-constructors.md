# Parsing strings as numbers

One design feature I like about Swift are the constructors on the
numeric data types that take a string: they are the equivalent of C#'s
`Parse` and `TryParse` methods, but since Swift supports constructors
that might fail, these can be used convenienntly like this:

```
var someString = "..."

if let number = Int (someString) {
    print ("\(number) plus 10 is \(number+10)")
}
```

