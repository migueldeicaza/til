I learned this the very hard way, and through a lot of pain, and mostly because
there is very little documentation around this.

But `.framework` files for iOS do not use the same layout as MacOS ones, in
particular, the entire `Versions/` directory with its `A`, `B` and so on
directories and the `Current` do not exist. 

For iOS, the expectation is that `Modules`, `Headers`, and `Resources` all live
in the top-level directory.   Failing to generate one of these by hand means
that later down the line you will get an obscure message from `codesign` that
"Bundle format unrecognized, invalid, or unsuitable." because it is trying to
sign an empty `Versions/A` directory.

I found this [blog
post](https://medium.com/walmartglobaltech/ios-library-bundle-and-frameworks-8fdc0aac6952)
that talked about this, but most importantly, [showed the layout](https://github.com/shilpabansal/StaticDynamicFramework) that you need.