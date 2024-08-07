To produce flame graphs using Xcode, you need to do a "Time Instrument"
profiler, and then profile your app as usual.  Then the important bit is to
select a thread and the root node in the callstack.

Online folks suggest to set "Separate by Thread" and "Invert call stack" (this
is from Google's page to convert this into another format).

Then you can select "Edit/Deep Copy", if you do not select the node, then
Instruments will keep that option greyed out.

Then you can save that into a file and use original [Flamegraph](https://github.com/brendangregg/FlameGraph), or use this [alternative
flamegraph](https://github.com/lennet/FlameGraph).

For the original flamegraph, you need to run a Perl tool to produce the flat
data:

```bash
$ stackcollapse-instruments.pl < xcode-deep-copy > flat-output
```

Then you can generate your Flamegraph:

```bash
$ perl flamegraph.pl --width 5000 flat-output > flame.svg
```


