Swift's new built-in regular expression is great, and I was today
hoping to replace some text with some other text, but none of the
public posts covered this - they share some clues, but not the
complete story, and it is quite neat.

You can use this snippet to replace the match with a decorator
for example:

```swift

let demo = "Question: demo"
mod = mod.replacing(/Question: (\w+)/, with: { match in match.output.1.uppercased ()})
```

Which in this case would replace `demo` with the uppercased version,
the various match groups are available as part of the match result.
