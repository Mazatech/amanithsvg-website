---
layout: default
title: "Data Types"
date: 2018-01-01 08:00:00 +0100
chapter: 1
categories: [api]
---

# Data Types

## Primitive data types

AmanithSVG defines a number of primitive data types by means of C typedefs.

### SVGTbyte

`SVGTbyte` defines an 8-bit two’s complement signed integer, which may contain values between `-128` and
`127`, inclusive.

### SVGTubyte

`SVGTubyte` defines an 8-bit unsigned integer, which may contain values between `0` and `255`, inclusive.

### SVGTshort

`SVGTshort` defines a 16-bit two’s complement signed integer, which may contain values between `-32768` and
`32767`, inclusive.

### SVGTint

`SVGTint` defines a 32-bit two’s complement signed integer.

### SVGTuint
`SVGTuint` defines a 32-bit unsigned integer.

### SVGTbitfield

`SVGTbitfiel` defines a 32-bit unsigned integer value, used for parameters that may combine a number of
independent single-bit values. A `SVGTbitfield` must be able to hold at least 32 bits.

### SVGTboolean

`SVGTboolean` is an enumeration that only takes on the values of `SVGT_FALSE (0)` or `SVGT_TRUE (1)`. Any nonzero
value used as a `SVGTboolean` will be interpreted as `SVGT_TRUE`.

```c
typedef enum {
    SVGT_FALSE = 0,
    SVGT_TRUE = 1
} SVGTboolean;
```

### SVGTfloat

`SVGTfloat` defines a 32-bit IEEE 754 floating-point value.

---


 | Data type name | Size | Values range |
 | -------------- | ---- | -------------|
 | `SVGTbyte` | `1 byte` | `[ -128, 127 ]` |
 | `SVGTubyte` | `1 byte` | `[ 0, 255 ]` |
 | `SVGTshort` | `2 byte` | `[ -32768, 32767 ]` |
 | `SVGTint` | `4 byte` | `[ -(2^31), 2^31 - 1 ]` |
 | `SVGTuint` | `4 byte` | `[ 0, 2^32 - 1 ]` |
 | `SVGTfloat` | `4 byte` | `IEEE 754 Standard` | 
 | `SVGTboolean` | `4 byte` | `[ SVGT_FALSE(0), SVGT_TRUE(1) ]` |
 | `SVGTbitfiled` | `4 byte` | `[ 0, 2^32 - 1 ]` |
{:.rwd-table2}

---
