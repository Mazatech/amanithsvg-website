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
/*
    Get renderer and version information.
    This function does not set the last-error code (see svgtGetLastError).

    If specified 'name' is not a valid value from the SVGTStringID
    enumeration, an empty string is returned.
*/
const char* svgtGetString(SVGTStringID name);
```

Using this function it is possible to get information about AmanithSVG version, OpenVG backend version and extensions, the list of compile-time options used to build the library.

# Log utilities

## Debug log

AmanithSVG can provide detailed information about error situations that can occur when parsing and rendering SVG content. For example AmanithSVG can report if a closing tag did not match the opening one (or if some tag was not closed at all) when parsing an SVG document; or if a font needed for rendering has not been set. In case of error (see `svgtGetLastError`), it is possible to receive additional information on the conditions (which caused the error) in the form of text messages that AmanithSVG can append to a given memory buffer.
The user must specify such memory buffer on a per-thread basis, so that each thread can log AmanithSVG warnings and errors without incurring in synchronism issues: the given memory buffer must be a valid writable memory area throughout the whole life of the thread.

```c
/*
    Set the log buffer for the current thread.
    It is important to not specify the same buffer memory for different
    threads, because AmanithSVG does not synchronize write operations to the
    provided log buffer: in other words, each thread must provide its own log
    buffer.

    'logBuffer' can be NULL or a valid pointer to a characters buffer where
    AmanithSVG can log messages. If NULL, logging is disabled. Messages will
    be appended one after the other, as long as there is enough free space
    within the buffer. When there is no more space, messages will stop being
    written to the buffer.

    'logBufferCapacity' is the capacity of 'logBuffer', in characters. If zero
    is specified, logging is disabled. AmanithSVG has no way of checking the
    actual buffer capacity, so it is up to the caller to specify the correct
    value, otherwise memory errors will be possible (e.g. buffer overrun).

    'logLevel' must be a bitwise OR of the desired SVGTLogLevel values.
    If zero is specified, logging is disabled.

    NB: after calling this function, the buffer will be initialized as empty
    (i.e. a '\0' character will be written at the beginning).

    This function returns:

    - SVGT_NOT_INITIALIZED_ERROR if the library has not yet been initialized
      (see svgtInit)

    - SVGT_ILLEGAL_ARGUMENT_ERROR if 'logLevel' is not a bitwise OR of values
      from the SVGTLogLevel enumeration
*/
SVGTErrorCode svgtLogBufferSet(char* logBuffer,
                               SVGTuint logBufferCapacity,
                               SVGTbitfield logLevel);
```

For example, by providing a malformed SVG content to the `svgtDocCreate` function, it is possible to receive detailed information on the specific parsing error:

```
[E] xml parsing: a closing tag did not match the opening one or some tag was not closed at all
Error offset: 11385
Error at: <rect width="587" height="412"></rct>
```

From an application point of view, it could be useful to apppend some custom messages to the AmanithSVG log buffer. One of the main purposes can be to insert debug messages, to better understand the interactions between the application and the AmanithSVG library. In this way the application can keep track of all SVG-related activities, and the whole log buffer can be analyzed later (if needed).

```c
/*
    Append the given message to the log buffer.
    Log buffer must have been already set through svgtLogBufferSet function,
    otherwise the message will be discarded.

    'message' must be a null-terminated string.

    'level' represents the message severity, and it determines its importance.

    This function returns:

    - SVGT_NOT_INITIALIZED_ERROR if the library has not yet been initialized
      (see svgtInit)

    - SVGT_ILLEGAL_ARGUMENT_ERROR if 'message' is null or if 'level' is not a
      value from the SVGTLogLevel enumeration

    - SVGT_NO_ERROR if no buffer has been set (via svgtLogBufferSet), or if
      the operation was completed successfully
*/
SVGTErrorCode svgtLogPrint(const char* message,
                           SVGTLogLevel level);
```

At any time the status of log buffer can be retrieved using the `svgtLogBufferInfo` function. In particular it is possible to know the current length (i.e. the total number of
characters written) and if it is full (i.e. AmanithSVG can no longer write any message to it).

```c
/*
    Get information about the current thread log buffer.
    The 'info' parameter must be an array of (at least) 4 entries, it will
    be filled with:

    - info[0] = log level (a bitwise OR of SVGTLogLevel enumeration)

    - info[1] = buffer capacity, in characters

    - info[2] = current length, in characters (i.e. the total number of
                characters written, included the trailing '\0')

    - info[3] = fullness (SVGT_TRUE if buffer is full, else SVGT_FALSE)

    NB: please note that buffer fullness does not correspond, in general, to
    the length == capacity condition. So in order to know if buffer is full
    (i.e. AmanithSVG can no longer write any messages to it), simply check
    the returned information in info[3] entry.

    This function returns:

    - SVGT_NOT_INITIALIZED_ERROR if the library has not yet been initialized
      (see svgtInit)

    - SVGT_ILLEGAL_ARGUMENT_ERROR if 'info' pointer is NULL or if it's not
      properly aligned
*/
SVGTErrorCode svgtLogBufferInfo(SVGTuint* info);
```

If the log buffer is full, it can be cleared and rewound by calling `svgtLogBufferSet` again (specifying the same pointer and capacity).
At any time the log buffer can be dumped (from the same thread that owns it) to an external file (or console), remembering that the reported length (i.e. `info[2]`) also includes the trailing '\0' character.

## Log levels

The `SVGTLogLevel` enumeration type defines which type of messages should be logged by AmanithSVG. One or more levels can be enabled at the same time (see `svgtLogBufferSet`) by simply bitwise OR values from the SVGTLogLevel enumeration.

| Log level | Notes |
| ---------- | ----- |
| `SVGT_LOG_LEVEL_ERROR` | Errors that prevent the rendering from being completed, such that malformed SVG (xml) content, negative dimensions for geometric shapes, and all those cases where a document fragment is technically in error according to SVG specifications |
| `SVGT_LOG_LEVEL_WARNING` | Warning conditions that need to be taken care of, such that an outermost `<svg>` element without a `width` or `height` attribute, a missing font resource, and so on |
| `SVGT_LOG_LEVEL_INFO ` | Informational messages |
| `SVGT_LOG_LEVEL_ALL` | All levels enabled at once |
{:.rwd-table .rwd-table-log-levels}

---
