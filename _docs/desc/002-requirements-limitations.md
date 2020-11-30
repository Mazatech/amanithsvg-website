---
layout: default
title: "Requirements and limitations"
date: 2018-01-01 08:00:00 +0100
chapter: 2
categories: [desc]
---

# Requirements and limitations

Starting from version 1.3.3, AmanithSVG is thread-safe: all the exposed functions can be called from multiple threads at the same time.
Implementation makes use of the following libraries to implement thread-safety:

- native Win32 calls on Windows platform
- pthreads (POSIX Threads) on MacOS X, Linux, Android, QNX
- NSLock / NSThread objects on iOS

There is a limitation in the maximum number of different threads that can use the library concurrently; this parameter can be configured at compile time and can be retrieved (at runtime) by using the `svgtMaxCurrentThreads` function.

AmanithSVG uses 32 bit single precision floating point arithmetic, as defined by IEEE 754-1985 standard, to perform some internal computations. All input data that describe geometries such as (but not limited to) path coordinates, paint parameters and matrices, will be converted from relative to absolute forms (if applicable), normalized (if applicable) and internally stored as a 32 bit floating point number. After these steps, it could result that different input data will become the same value under the 32 bit floating point precision.

AmanithSVG uses fixed point arithmetic to perform the rasterization; in this case internal rendering surface coordinates are represented using 16bit. Fixed point format is 12.4 (12 bits for the integer part, 4 bits for the fractional part): supported maximum rendering surface dimension is 4096 pixels. In other terms, AmanithSVG supports rendering surfaces with dimensions less than or equal to 4096 pixels only.

---
