---
layout: default
title: "SVG packing and atlas generation"
date: 2018-01-01 08:00:00 +0100
chapter: 8
categories: [api]
---

# Packing of SVG documents and atlas generation

## Packing of SVG documents and atlas generation

Besides rendering single SVG documents over drawing surfaces, AmanithSVG can pack the rendering of one or more SVG documents within one or more drawing surfaces, automatically. For each given SVG, you can choose to pack the whole document or instead to pack each first-level element separately.

The whole process consists of some steps that must be followed in this order:

 - start the packing process using the `svgtPackingBegin` function

 - add one or more SVG document to the process, using the `svgtPackingAdd` function

 - stop the packing process using the `svgtPackingEnd` function

 - get the number of generated "atlas data structures" using the `svgtPackingBinsCount` function

 - for each atlas, get its information (e.g. its dimensions in pixels) using the `svgtPackingBinInfo` function, then:
  - create a drawing surface with the same atlas dimensions, using the `svgtSurfaceCreate` function
	- get the list of SVG documents/elements packed in the current atlas, using the `svgtPackingBinRects` function
	- draw the SVG documents/elements packed in the current atlas, using the `svgtPackingDraw` function

Here is a complete example code, that will be discussed in detail within next sections:

```c
SVGTuint i, atlasesCount;
SVGTHandle docHandles[DOCS_COUNT];

/* load SVG documents */
for (i = 0; i < DOCS_COUNT; ++i) {
    docHandles[i] = loadSvg(fileNames[i]);
}

/* set a transparent black clear color and enable surface clearing */
svgtClearColor(0.0f, 0.0f, 0.0f, 0.0f);
svgtClearPerform(SVGT_TRUE);

/* initialize the packing process: generate atlases with a maximum
   1024 pixels dimension and no additional scale */
svgtPackingBegin(1024, 1, SVGT_FALSE, 1.0f);

/* add each SVG document to the packer */
for (i = 0; i < DOCS_COUNT; ++i) {
    SVGTuint info[2];
    /* add the SVG, specifying to pack each first-level element separately */
    svgtPackingAdd(docHandles[i], SVGT_TRUE, 1.0f, info);
}

/* finish the packing process */
svgtPackingEnd(SVGT_TRUE);

/* get the number of generated atlas structures */
atlasesCount = svgtPackingBinsCount();

/* loop over atlases */
for (i = 0; i < atlasesCount; ++i) {
    SVGTuint atlasInfo[3];
    SVGTHandle surface;
    /* get atlas width, height and the number of packed documents/elements */
    svgtPackingBinInfo(i, atlasInfo);
    /* create a drawing surface with the same atlas dimension */
    surface = svgtSurfaceCreate(atlasInfo[0], atlasInfo[1]);
    /* draw all documents/elements over the drawing surface */
    svgtPackingDraw(i, 0, atlasInfo[2], surface, SVGT_RENDERING_QUALITY_BETTER);
    /* if needed, process the drawing surface (e.g. upload its pixels
    on a GPU texture) and then destroy it */
    < do something with surface pixels (e.g. upload pixels to a GPU texture) >
    /* destroy the surface */
    svgtSurfaceDestroy(surface);
}
```

---

## Start a packing process

A packing process must be initialized through the `svgtPackingBegin` function:

```c
/*
    Start a packing task: one or more SVG documents will be collected
    and packed into bins, for the generation of atlases.

    Every collected SVG document/element will be packed into rectangular bins,
    whose dimensions won't exceed the specified 'maxDimension', in pixels.

    If SVGT_TRUE, 'pow2Bins' will force bins to have power-of-two dimensions.

    Each rectangle will be separated from the others by the specified 'border'
    in pixels.

    The specified 'scale' factor will be applied to all collected SVG
    documents/elements, in order to realize resolution-independent atlases.

    NB: floating-point values of NaN are treated as 0, values of +Inf and -Inf
    are clamped to the largest and smallest available float values.

    This function returns:

    - SVGT_STILL_PACKING_ERROR if a current packing task is still open

    - SVGT_ILLEGAL_ARGUMENT_ERROR if specified 'maxDimension' is 0

    - SVGT_ILLEGAL_ARGUMENT_ERROR if 'pow2Bins' is SVGT_TRUE and the
      specified 'maxDimension' is not a power-of-two number

    - SVGT_ILLEGAL_ARGUMENT_ERROR if specified 'border' itself would exceed
      the specified 'maxDimension' (border must allow a packable region of
      at least one pixel)

    - SVGT_ILLEGAL_ARGUMENT_ERROR if specified 'scale' factor is less than or
      equal 0

    - SVGT_NO_ERROR if the operation was completed successfully
*/
SVGTErrorCode svgtPackingBegin(SVGTuint maxDimension,
                               SVGTuint border,
                               SVGTboolean pow2Bins,
                               SVGTfloat scale);
```

If generated atlases are going to be uploaded on GPU textures (OpenGL/Direct3D), the `maxDimension` and `pow2Bins` parameters should match the relative GPU capabilities (maximum texture dimension and the support of rectangular/NPOT textures).

---

## Add SVG documents to the current packing process

Once that a packing process has been started, one or more SVG documents can be added using the `svgtPackingAdd` function:

```c
/*
    Add an SVG document to the current packing task.

    If SVGT_TRUE, 'explodeGroups' tells the packer to not pack the whole
    SVG document, but instead to pack each first-level element separately.

    The additional 'scale' is used to adjust the document content to the
    other documents involved in the current packing process.

    The 'info' parameter will return some useful information, it must be
    an array of (at least) 2 entries and it will be filled with:

    - info[0] = number of collected bounding boxes

    - info[1] = the actual number of packed bounding boxes (boxes whose
    dimensions exceed the 'maxDimension' value specified to the
    svgtPackingBegin function, will be discarded)

    NB: floating-point values of NaN are treated as 0, values of +Inf and -Inf
    are clamped to the largest and smallest available float values.

    This function returns:

    - SVGT_NOT_PACKING_ERROR if there isn't a currently open packing task

    - SVGT_BAD_HANDLE_ERROR if specified document handle is not valid

    - SVGT_ILLEGAL_ARGUMENT_ERROR if specified 'scale' factor
      is less than or equal 0

    - SVGT_ILLEGAL_ARGUMENT_ERROR if 'info' pointer is NULL or
      if it's not properly aligned

    - SVGT_NO_ERROR if the operation was completed successfully
*/
SVGTErrorCode svgtPackingAdd(SVGTHandle svgDoc,
                             SVGTboolean explodeGroups,
                             SVGTfloat scale,
                             SVGTuint* info);
```

Information returned by the `info` parameter is particularly useful when `explodeGroups` flag is `SVGT_TRUE` (i.e. first-level elements will be packed separately): if the number of collected elements bounding boxes (`info[0]`) is different (i.e. greater) than the actual number of packed bounding boxes (`info[1]`), it means that the `maxDimension` value (in conjunction with the `scale` parameter) specified to the `svgtPackingBegin` function is not enough to pack one or more SVG elements (relative to the given document).

In this case, the application can choose to inform the user and/or abort the packing process (see the next chapter). If desired, a new packing process could be restarted by calling `svgtPackingBegin` with a bigger `maxDimension` value (or a smaller `scale` parameter).

---

## Stop a packing process

After adding all the desired SVG documents/elements to the current packing process, it is possible to finish it using the following function:

```c
/*
    Close the current packing task and, if specified, perform the real packing
    algorithm.

    All collected SVG documents/elements (actually their bounding boxes) are
    packed into bins for later use (i.e. atlases generation).
    After calling this function, the application could use svgtPackingBinsCount,
    svgtPackingBinInfo and svgtPackingDraw in order to get information about the
    resulted packed elements and draw them.

    This function returns:

    - SVGT_NOT_PACKING_ERROR if there isn't a currently open packing task

    - SVGT_NO_ERROR if the operation was completed successfully
*/
SVGTErrorCode svgtPackingEnd(SVGTboolean performPacking);
```

If the `performPacking` flag is set to `SVGT_FALSE`, the whole packing process is aborted, and all collected information is simply discarded: if desired, a new packing process could be restarted by calling `svgtPackingBegin`. If instead the flag is set to `SVGT_TRUE`, the real packing algorithm will be ran over the collected SVG elements.

---

## Generate atlases (draw packed elements)

Once that a packing process has been executed successfully, the number of generated atlases data structures (i.e. the structure that contains, for each atlas, its dimension in pixels and the list of packed SVG elements belonging to it) can be retrieved through the following function:

```c
/*
    Return the number of generated bins from the last packing task.

    This function returns a negative number in case of errors (e.g. if the
    current packing task has not been previously closed by a call to
    svgtPackingEnd).
*/
SVGTint svgtPackingBinsCount(void);
```

All the information relative to a single atlas data structure can be achieved using the following functions:

```c
/*
    Return information about the specified bin.

    The requested bin is selected by its index; the 'binInfo' parameter must
    be an array of (at least) 3 entries, it will be filled with:

    - binInfo[0] = bin width, in pixels

    - binInfo[1] = bin height, in pixels

    - binInfo[2] = number of packed rectangles within the bin

    This function returns:

    - SVGT_STILL_PACKING_ERROR if a current packing task is still open

    - SVGT_ILLEGAL_ARGUMENT_ERROR if specified 'binIdx' is not valid(must be
      >= 0 and less than the value returned by svgtPackingBinsCount function)

    - SVGT_ILLEGAL_ARGUMENT_ERROR if 'binInfo' pointer is NULL or if
      it's not properly aligned

    - SVGT_NO_ERROR if the operation was completed successfully
*/
SVGTErrorCode svgtPackingBinInfo(SVGTuint binIdx,
                                 SVGTuint* binInfo);
```

```c
/*
    Get access to packed rectangles, relative to a specified bin.

    The specified 'binIdx' must be >= 0 and less than the value returned by
    svgtPackingBinsCount function, else a NULL pointer will be returned.

    The returned pointer contains an array of packed rectangles, whose
    number is equal to the one gotten through the svgtPackingBinInfo function.

    The use case for which this function was created, it's to copy and cache
    the result of a packing process; then when needed (e.g. requested by the
    application), the rectangles can be drawn using the svgtPackingRectsDraw
    function.
*/
const SVGTPackedRect* svgtPackingBinRects(SVGTuint binIdx);
```

Before to explain the `SVGTPackedRect` structure in detail, it is useful to show how to draw packed SVG elements. It is important to understand that each generated atlas data structure should correspond to a drawing surface (with the same dimensions), where packed elements will be drawn onto. The function used to draw packed elements, relative to a given atlas data structure, is the following:

```c
/*
    Draw a set of packed SVG documents/elements over the specified drawing
    surface.

    The drawing surface is cleared (or not) according to the current settings
    (see svgtClearColor and svgtClearPerform). After calling svgtPackingEnd, the
    application could use this function in order to draw packed elements before
    to start another packing task with svgtPackingBegin.

    This function returns:

    - SVGT_ILLEGAL_ARGUMENT_ERROR if specified 'binIdx' is not valid (must be
      >= 0 and less than the value returned by svgtPackingBinsCount function)

    - SVGT_ILLEGAL_ARGUMENT_ERROR if specified 'startRectIdx', along with
      'rectsCount',identifies an invalid range of rectangles; defined:

        - maxCount = binInfo[2] (see svgtPackingBinInfo)

        - endRectIdx = 'startRectIdx' + 'rectsCount' - 1

    it must be ensured that 'startRectIdx' < maxCount and
    'endRectIdx' < maxCount, else SVGT_ILLEGAL_ARGUMENT_ERROR is returned.

    - SVGT_BAD_HANDLE_ERROR if specified surface handle is not valid

    - SVGT_NO_ERROR if the operation was completed successfully
*/
SVGTErrorCode svgtPackingDraw(SVGTuint binIdx,
                              SVGTuint startRectIdx,
                              SVGTuint rectsCount,
                              SVGTHandle surface,
                              SVGTRenderingQuality renderingQuality);
```

So, for example, in order to draw the elements belonging to the first generated atlas structure, the following code could be used:

```c
SVGTuint atlasInfo[3];
SVGTHandle surface;
/* get atlas width, height and the number of packed documents/elements */
svgtPackingBinInfo(0, atlasInfo);
/* create a drawing surface with the same atlas dimension */
surface = svgtSurfaceCreate(atlasInfo[0], atlasInfo[1]);
/* draw all documents/elements over the drawing surface */
svgtPackingDraw(0, 0, atlasInfo[2], surface, SVGT_RENDERING_QUALITY_BETTER);
```

Now it is important to introduce the `SVGTPackedRect` structure:

```c
typedef struct {
    /* 'id' attribute, NULL if not present. */
    const char* elemName;
    /* Original rectangle corner. */
    SVGTint originalX;
    SVGTint originalY;
    /* Rectangle corner position. */
    SVGTint x;
    SVGTint y;
    /* Rectangle dimensions. */
    SVGTint width;
    SVGTint height;
    /* SVG document handle. */
    SVGTHandle docHandle;
    /* 0 for the whole SVG, else the element (tree) index. */
    SVGTuint elemIdx;
    /* Z-order. */
    SVGTint zOrder;
    /* The used destination viewport width (induced by packing scale factor). */
    SVGTfloat dstViewportWidth;
    /* The used destination viewport height (induced by packing scale factor).*/
    SVGTfloat dstViewportHeight;
} SVGTPackedRect;
```

The `SVGTPackedRect` structure itself is not needed for the rendering of packed SVG elements, but it contains useful information that could be used by the application for different purposes. In particular, the fields `x`, `y`, `width`, `height` identify the exact SVG element location within the drawing surface (i.e. the location where the element has been rendered); if the element has an id attribute within the SVG document (e.g. a mnemonic name), such value is reported through the `elemName` field. The `zOrder` field represents the position of the element within its SVG document (i.e. the order in which it appears within the XML).

For example, lets consider the following SVG (`orc.svg`):

```xml
<?xml version="1.0" encoding="utf-8"?>
<svg version="1.1" id="Layer_1" xmlns="http://www.w3.org/2000/svg"
xmlns:xlink="http://www.w3.org/1999/xlink" x="0px" y="0px" width="145.925px"
height="264.76px" viewBox="0 0 145.925 264.76">
<g id="sx_arm_down">...</g>
<g id="sx_leg_up">...</g>
<g id="sx_leg_down">...</g>
<g id="dx_leg_up">...</g>
<g id="dx_leg_down">...</g>
<g id="sx_arm_up">...</g>
<g id="dx_arm_up">...</g>
<g id="body">...</g>
<g id="dx_arm_down">...</g>
<g id="head">...</g>
</svg>
```

| &nbsp; |
| :---: |
| *orc.svg* |
{:.tbl_images .orcSvg}

The packing of first-level elements (`maxDimension = 512, border = 1, pow2Bins = SVGT_FALSE, scale = 1`) will produce a single `452 x 108` atlas, with the following packed rectangles:

|          | elemName | originalX | originalY | x   | y   | width | height | zOrder |
| -------- | -------- | --------- | --------- | --- | --- | ----- | ------ | ------ |
| rects[0] | head | 5 | 9 | 0 | 0 | 133 | 105 | 9 |
| rects[1] | body | 26 | 80 | 133 | 0 | 93 | 97 | 7 |
| rects[2] | sx_leg_down | 75 | 191 | 226 | 0 | 48 | 61 | 2 |
| rects[3] | dx_leg_down | 41 | 191 | 274 | 0 | 38 | 63 | 4 |
| rects[4] | sx_arm_up | 95 | 102 | 312 | 0 | 38 | 54 | 5 |
| rects[5] | dx_arm_up | 19 | 98 | 350 | 0 | 38 | 54 | 6 |
| rects[6] | dx_arm_down | 19 | 141 | 388 | 0 | 32 | 68 | 8 |
| rects[7] | sx_arm_down | 99 | 139 | 420 | 0 | 32 | 68 | 0 |
| rects[8] | dx_leg_up | 43 | 147 | 312 | 54 | 38 | 54 | 3 |
| rects[9] | sx_leg_up | 70 | 146 | 350 | 54 | 38 | 54 | 1 |
{:.rwd-table2 .rwd-table-orcAtlas}

| &nbsp; |
| :---: |
| *orc atlas* |
{:.tbl_images .orcSvgAtlas}

It is important to note that the whole packing process, as well as the `svgtPackingDraw` function, is not affected by SVG documents viewports nor by drawing surfaces viewports. In other words, the viewport values set through `svgtDocViewportSet` and `svgtSurfaceViewportSet` functions do not affect the behavior of packing processes and atlas generation.

---
