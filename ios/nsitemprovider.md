The various NSItemProvider APIs to load data out of them might look like they
return the data synchronously, but they do not, and it was not clear to me that
this was just a friendly callback, but something that would be called into the
future.

This makes processing of various files in drag and drop painful, and took me
a lot longer to realized than I hoped.

I fell into the trap, because I started with `.dragging` modifier, that worked
great, but you can only use a single type in the drop operation, two types would
break.   So wasted that time.

Then I tried `.onDrop` which allows more than one data type, that works, but you
do not get the location of the drop.

Then I found the `.onDrop` that gets a delegate, and that one calls
`performDrop` with an array of `NSItemProviders` in the `DropInfo`.   And each
method here comes into play to do an async resolution.

I googled online and some folks create a `DispatchGroup`, and then followed by
an `enter`, `leave` and `wait`, which works for some data (like URL), but fails
for other things like strings, as it deadlocks - too tired to figure out why.

So in the end, I queued all the operations to resolve, accumultate the data, and
then inject the results when I am done.   Considering that they might get queued
direclty into a queue for execution rather than started after the drop event
finishes, I decided to play it safe and track every queue and resolution.