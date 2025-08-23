If you add an `.onTapGesture(count:2)` to a view that already has an
`.onTapGesture` handler, then the regulr tap actions will be delayed.

In retrospect this makes sense, but it caught me by surprise.