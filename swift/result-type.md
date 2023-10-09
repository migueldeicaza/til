# Some Exception Delights

One of my principles when designing software is that errors presented to the
user should be as informative as possible - to empower users to find a solution
to the problem they are facing and reducing the support and external assitance
they need to use your software.

This is easier said that done, and I have myself betrayed this principle that I
aspired to live up to, way too many times.

I have been going back and forth between using `Result<Success,Failure>` as a
return value, or throwing exceptions.

With result, I get to write code that always handles errors, like
this:

```swift
func myCall () -> Result<String,MyError> { ... }

switch myCall () {
    case .success(let result):
        print ("Got a valid result: \(result)")
    case .failure (let err):
        print ("failed, with code: \(err)")
    }
}
```

I have generally been avoiding throwing errors, mostly due to my C# baggage,
where exceptions are considered very costly to throw, but I am finally coming to
terms with it.   

At first, I hated that I needed to define custom errors.  This is a departure
from C# where I could usually reuse an exciting exception type to return errors.
So I avoided them at first.

When I had to use exceptions, I was in camp "Let us use try? and treat nulls as
errors", which is convenient, but you start to swallow valuable information.

I have recently started to enjoy not just using exceptions, but propagating my
exceptions results upwards.  That way, I have found that I am doing the work to
empower my users to solve their own problems.

Usually, a low-level routine would define its own error, and I make sure to
provide an explanation for those situations:

```swift
enum ConnectionError: Error {
    case timeout
    case unknownHost(String)

    public var description: String {
	switch self {
		case .timeout:
			return "Connection timed out"
		case .unknownHost(let name):
			return "It was not possible to resolve the hostname \(name)"
	}
    }
}

enum FetchMailError: Error {
	case connectionError(ConnectionError)
	case invalidCredential(String)

	public var description: String {
		switch self {
			case .connectionError(let e):
				return "There was a connection error: \(e)"
			case .invalidCredential(let user):
				return "Could not log in as \(user)"
		}
	}
}
```

That way, when the error is presented to the user, all of the information is
contained in the exception.

That said, sometimes I feel that the throwing syntax can be a little bit
offputing when writing code, so I still like to use Result in those cases.

Today I found myself wishing that there was a way of bridging these worlds, and
lo and behold, there is!

Just call the `get()` method on the result, and you can turn a Result into an exception:

```swift
let v = try myCall().get()
```
