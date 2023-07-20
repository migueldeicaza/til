# Double to Int casts in Swift can throw an exception

I have been chasing this bug for a long time, but could not figure out what was
wrong.   In recent versions of Swift, I finally got a crash report with the 
information.  So here in case it helps a poor soul in the future.

In Swift, casting a Double to an Int can throw an exception if you pass either
an infinite value or a NaN value.   So the following code would crash:

```
// Creates a +Inf value:
let x = 1.0/0.0

// try to cast it to an int:
let intX = Int (x)
// crashes here

// Create a NaN value:
let nan = 0.0/0.0

// this also crashes
let intNan = Int (nan)

```

So you need to check for these values before casting.  Something like this might
work, in this case, I map inifinite values to the largest Int value, and NaN to
a user provided-value, you might not need some of these cases

```
extension Double {
	func toIntClamped (nanValue: Int) -> Int {
		if self.isNaN {
			return nanValue
		}
		if self.isInfinite {
			if self > 0 {
				return max
			} else {
				return min
			}
		}
		return Int (self)
	}
}
```

Notice that C# behaves differently.   C# catches at compile time both division
by zero (so you can not create easily the infinite value or NaN value), but does
not perform any checks at runtime (which also happens to be the scenario that
was biting me).

In C# you can create the infinite and nan values like this:

```
double zero = 0
double inf = 1.0 / zero
double nan = 0 / zero
```

Or you can use the convenience properties in the Double class.

And while C# catches an attempt to cast a `Double.NaN` at compile time:

```
int x = (int) Double.NaN;
// error CS0221: Constant value `NaN' cannot be converted to a `int' (use `unchecked' syntax to override)
```

Merely passing the constants will silently produce odd results:

```
var a = (int) inf;	// Produces -2147483648
var b = (int) nan;	// Produces -2147483648
```