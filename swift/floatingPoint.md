# FloatingPoint protocol

The FloatingPoint protocol in Swift allows you to write code that is suitable
for the different floating point types.   I know, it does sound obvious in
retrospect, but I have been working blindly without acknowledging this, and was
missing out on all the opportunities to write generic floating point code, like
this:

```
public extension FloatingPoint {
    static func lerp(a: Self, b: Self, t: Self) -> Self {
        let one = Self(1)
        let oneMinusT = one - t
        let aTimesOneMinusT = a * oneMinusT
        let bTimesT = b * t
        return aTimesOneMinusT + bTimesT
    }
}
```