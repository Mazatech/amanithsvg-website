---
layout: default
title: "Requirements and limitations"
date: 2018-01-01 08:00:00 +0100
chapter: 2
categories: [desc]
---

# Requirements and limitations

AmanithSVG is a single thread library and it's not thread safe.

AmanithSVG uses 32 bit single precision floating point arithmetic, as defined by IEEE 754-1985 standard, to
perform some internal computations. All input data that describe geometries such as (but not limited to) path
coordinates, paint parameters and matrices, will be converted from relative to absolute forms (if applicable),
normalized (if applicable) and internally stored as a 32 bit floating point number. After these steps, it could
result that different input data will become the same value under the 32 bit floating point precision.

AmanithSVG uses fixed point arithmetic to perform the rasterization; in this case internal rendering surface
coordinates are represented using 16bit. Fixed point format is 12.4 (12 bits for the integer part, 4 bits for the
fractional part): supported maximum rendering surface dimension is 4096 pixels. In other terms,
AmanithSVG supports rendering surfaces with dimensions less than or equal to 4096 pixels only.

---
