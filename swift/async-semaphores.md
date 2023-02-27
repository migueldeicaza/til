# Semaphores in the Async world of Swift

I found myself inside a Swift async context needing the main thread to complete
a user interaction, and only resume execution after this was completed.

In the pre-async era, I would have used a convenient a semaphore to wait until
the user interface had completed, but this is not allowed in Swift, where all
[tasks are supposed to continue making forward
progress](https://developer.apple.com/videos/play/wwdc2021/10254/).

This means that code like this will likely lock your program:

```swift
func letUserChoose () async -> Bool {
    var choice: Bool = false
    let semaphore = DispatchSemaphore(value: 0)
    DispatchQueue.main.async {
	choice = presentChooiceUI ()
	semaphore.signal()
    }
    semaphore.wait()
    return choice
}
```

The reason is that the `wait()` call will block the current task, and it wont be
available for other users to use the thread.   Swift's new concurrency model is
such that it only keeps one thread per CPU core and does not spawn threads like
a drunken sailor would in demand for more resources.

The solution is to create a continuation that can later be resumed by the main
thread to return the result, like this:

```swift
func letUserChoose () async -> Bool { 
   let result: Bool = await withCheckedContinuation { c in 
      DispatchQueue.main.async { 
	choice = presentChooiceUI ()
	c.resume(returning: choice)
      } 
   } 
   return result
}
```

A conceptual use of this with SwiftUI is more or less this:

```swift
struct ContentView: View {
    @State var showDialog: Bool = false
    @State var postResult: CheckedContinuation<Bool, Never>? = nil

    var body: some View {
	VStack {
		Text ("Copying files...")
	}.onSheet (isPresented: $showDialog) {
		VStack {
			Text ("Disk is not empty, do you want to format it?")
			Button("Yes") {
				postResult.resume(returning: true)
			}
			Button("No") {
				postResult.resume(returning: false)
			}
		}
	}
	.task {
		// Start background task
		if !diskIsEmpty () {
			let result = await withCheckedContinuation { c in
				DispatchQueue.main.async {
					postResult = c
					showDialog = true
				}
			}
			if !result {
				// We wont format the disk
			}
		}
		formatDisk ()
	}
    }
}
```

Some folks shared their general purpose solutions, like this [pretty one from
Sebastian
Toivonen](https://forums.swift.org/t/semaphore-alternatives-for-structured-concurrency/59353/3):

```swift
actor Semaphore {
    private var count: Int
    private var waiters: [CheckedContinuation<Void, Never>] = []

    init(count: Int = 0) {
        self.count = count
    }

    func wait() async {
        count -= 1
        if count >= 0 { return }
        await withCheckedContinuation {
            waiters.append($0)
        }        
    }

    func release(count: Int = 1) {
        assert(count >= 1)
        self.count += count
        for _ in 0..<count {
            if waiters.isEmpty { return }
            waiters.removeFirst().resume()
        }
    }
}
```

And some folks have built complete libraries for this, like [AsyncObjects](https://github.com/SwiftyLab/AsyncObjects).

