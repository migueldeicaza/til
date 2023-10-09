The `SIMD3<Float>` data type does not use 12 bytes, but uses 16 bytes instead,
as it is a convenience wrapper for the SIMD hardware on the system.

This is just an issue when you want to interop with code that assumed packed
3-float values.  So you will want to flatten that stuff.