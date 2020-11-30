---
layout: default
title: "Miscellaneus utilities"
date: 2018-01-01 08:00:00 +0100
chapter: 9
categories: [api]
---

# Miscellaneous utilities

In addition to the features already described, AmanithSVG exposes some utility functions that could result useful in some situations.

```c
/*
    Copy drawing surface content into the specified pixels buffer.

    This function is useful for managed environments (e.g. C#, Unity, Java,
    Android), where the use of a direct pixels access (i.e. svgtSurfacePixels)
    is not advisable nor comfortable.

    If the 'redBlueSwap' flag is set to SVGT_TRUE, the copy process will also
    swap red and blue channels for each pixel; this kind of swap could be useful
    when dealing with OpenGL/Direct3D texture uploads (RGBA or BGRA formats).

    If the 'dilateEdgesFix' flag is set to SVGT_TRUE, the copy process will also
    perform a 1-pixel dilate post-filter; this dilate filter could be useful
    when surface pixels will be uploaded to OpenGL/Direct3D bilinear-filtered
    textures.

    This function returns:

    - SVGT_BAD_HANDLE_ERROR if the specified surface handle is not valid

    - SVGT_ILLEGAL_ARGUMENT_ERROR if 'dstPixels32' pointer is NULL or if
      it's not properly aligned

    - SVGT_NO_ERROR if the operation was completed successfully
*/
SVGTErrorCode svgtSurfaceCopy(SVGTHandle surface,
                              void* dstPixels32,
                              SVGTboolean redBlueSwap,
                              SVGTboolean dilateEdgesFix);
```

Each drawing surface pixel is a 32bit integer number, represented as a packed `ARGB` value (A = MSB, B = LSB on little endian machines): this format, on little endian machines, corresponds to OpenGL `GL_BGRA` external texture format (`GL_IMG_texture_format_BGRA8888` / `GL_EXT_texture_format_BGRA8888` / `GL_EXT_bgra` / `GL_APPLE_texture_format_BGRA8888` extensions), `D3DFMT_A8R8G8B8` / `D3DFMT_X8R8G8B8` Direct3D 9 texture formats, `DXGI_FORMAT_B8G8R8A8_UNORM` Direct3D 11 format, `MTLPixelFormatBGRA8Unorm` / `MTLPixelFormatBGRA8Unorm_sRGB` Metal format.

If surface pixels are going to be uploaded to GPU textures, it could be useful to use `svgtSurfaceCopy` function to perform (if required by the GPU capabilities) the swap between red and blue channels, and/or at the same time to perform a 1-pixel dilate filter to enhance visual quality on bilinear filtered textures. Here is a possible example of use:

```c
SVGTuint surfaceWidth = svgtSurfaceWidth(surface);
SVGTuint surfaceHeight = svgtSurfaceHeight(surface);
SVGTuint* tmpPixels = (SVGTuint*)malloc(surfaceWidth * surfaceHeight *
                                        sizeof(SVGTuint));
/* copy surface pixels, applying a 1-pixel dilate filter (no red-blue channels swap) */
svgtSurfaceCopy(surface, tmpPixels, SVGT_FALSE, SVGT_TRUE);
< upload tmpPixels to the GPU, using your preferred 3D API >
free(tmpPixels);
```

```c
/*
    Map a point, expressed in the document viewport system, into the surface
    viewport. The transformation will be performed according to the current
    document viewport (see svgtDocViewportGet) and the current surface viewport
    (see svgtSurfaceViewportGet).

    The 'dst' parameter must be an array of (at least) 2 float entries, it will
    be filled with:

    - dst[0] = transformed x

    - dst[1] = transformed y

    NB: floating-point values of NaN are treated as 0, values of +Inf and -Inf
        are clamped to the largest and smallest available float values.

    This function returns:

    - SVGT_BAD_HANDLE_ERROR if specified document (or surface) handle is not
      valid

    - SVGT_ILLEGAL_ARGUMENT_ERROR if 'dst' pointer is NULL or if it's not
      properly aligned

    - SVGT_NO_ERROR if the operation was completed successfully
*/
SVGTErrorCode svgtPointMap(SVGTHandle svgDoc,
                           SVGTHandle surface,
                           SVGTfloat x,
                           SVGTfloat y,
                           SVGTfloat* dst);
```

The combined use of `svgtDocViewportSet` and `svgtSurfaceViewportSet` induces a transformation matrix, that will be used to draw (see `svgtDocDraw`) an SVG document.

The induced matrix grants the document viewport to be mapped onto the surface viewport (respecting the specified alignment): all SVG content will be drawn by the `svgtDocDraw` function accordingly.

The `svgtPointMap` function can be used to calculate the drawing surface position that will take a point belonging to the SVG document viewport.

```c
/*!
    Get renderer and version information.
*/
const char* svgtGetString(SVGTStringID name);
```

Using this function it is possible to get information about AmanithSVG version, OpenVG backend version and extensions, the list of compile-time options used to build the library.

---
