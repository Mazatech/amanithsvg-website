---
layout: default
title: "Create a drawing surface"
date: 2018-01-01 08:00:00 +0100
chapter: 6
categories: [api]
---

# Create a drawing surface

## Create a drawing surface

SVG documents can be drawn on surfaces (a "rectangular" memory region made of pixels). Each pixel is a 32bit integer number, represented as a packed ARGB value (A = Most Significant Byte, B = Least Significant Byte on little endian machines). A drawing surface can be created through the `svgtSurfaceCreate` function:

```c
/*
    Create a new drawing surface, specifying its dimensions in pixels.
    Specified width and height must be greater than zero; they are silently
    clamped to the value returned by the svgtSurfaceMaxDimension function.

    The user should call svgtSurfaceWidth and svgtSurfaceHeight after
    svgtSurfaceCreate in order to check real drawing surface dimensions.

    Return SVGT_INVALID_HANDLE in case of errors, else a valid drawing surface
    handle.
*/
SVGTHandle svgtSurfaceCreate(SVGTuint width, SVGTuint height);
```

```c
/*
    Get the maximum dimension allowed for drawing surfaces.

    This is the maximum valid value that can be specified as 'width' and
    'height' for the svgtSurfaceCreate and svgtSurfaceResize functions.

    In order to get a valid value, the library must have been already
    initialized (see svgtInit).
*/
SVGTuint svgtSurfaceMaxDimension(void);
```

After a drawing surface has been created, it is possible to retrieve its dimensions (in pixels) and a pointer to its pixels using the following functions:

```c
/*
    Get width dimension (in pixels), of the specified drawing surface.
    If the specified surface handle is not valid, 0 is returned.
*/
SVGTuint svgtSurfaceWidth(SVGTHandle surface);
```

```c
/*
    Get height dimension (in pixels), of the specified drawing surface.
    If the specified surface handle is not valid, 0 is returned.
*/
SVGTuint svgtSurfaceHeight(SVGTHandle surface);
```

```c
/*
    Get access to the drawing surface pixels.
    If the specified surface handle is not valid, NULL is returned.

    Please use this function to access surface pixels for read-only purposes
    (e.g. blit the surface on the screen according to the platform graphic
    subsystem, upload pixels into a GPU texture, and so on).

    Writing or modifying surface pixels by hand is still possible, but NOT
    ADVISABLE.
*/
const void* svgtSurfacePixels(SVGTHandle surface);
```

When it is no longer needed, a surface can be destroyed using the `svgtSurfaceDestroy` function:

```c
/*
    Destroy a previously created drawing surface.

    Returns SVGT_NO_ERROR if the operation was completed successfully,
    else an error code.
*/
SVGTErrorCode svgtSurfaceDestroy(SVGTHandle surface);
```

## Clear a drawing surface

Before to draw SVG documents, it is highly recommended to clear the drawing surface with a given color:

```c
/*
    Clear the whole drawing surface with the given color.
    Each color component must be a number between 0 and 1. Values outside this
    range will be clamped.

    It returns SVGT_NO_ERROR if the operation was completed successfully, else
    an error code.

    NB: floating-point values of NaN are treated as 0, values of +Infinity and
    -Infinity are clamped to the largest and smallest available float values.
*/
SVGTErrorCode svgtSurfaceClear(SVGTHandle surface,
                               SVGTfloat r,
                               SVGTfloat g,
                               SVGTfloat b,
                               SVGTfloat a);
```

---
