---
layout: default
title: "Rendering SVG documents"
date: 2018-01-01 08:00:00 +0100
chapter: 7
categories: [api]
---

# Rendering SVG documents

## Rendering SVG documents

Once that an SVG document has been created, it can be rendered over a drawing surface using the following function:

```c
/*
    Draw an SVG document, over the specified drawing surface, with the given quality.
    If the specified SVG document is SVGT_INVALID_HANDLE, the drawing surface is cleared
    (or not) according to the current settings (see svgtClearColor and svgtClearPerform)
    and nothing else is drawn.

    Return SVGT_NO_ERROR if the operation was completed successfully, else an error code.
*/
SVGTErrorCode svgtDocDraw(SVGTHandle svgDoc,
                          SVGTHandle surface,
                          SVGTRenderingQuality renderingQuality);
```

The third parameter influences the quality (and speed) of the drawing process, it can be one of the following values:

 - `SVGT_RENDERING_QUALITY_NONANTIALIASED`: it disables antialiasing at all.

 - `SVGT_RENDERING_QUALITY_FASTER`: it causes rendering to be done at the highest available speed, while still performing (low quality) edge antialiasing.

 - `SVGT_RENDERING_QUALITY_BETTER`: it causes rendering to be done with the highest available quality.

Every time that `svgtDocDraw` is called, before to perform the requested rendering, the drawing surface will be cleared (or not) according to the current state induced by these two functions:

```c
/*
    Set the clear color (i.e. the color used to clear the whole drawing surface).
    Each color component must be a number between 0 and 1.
    Values outside this range will be clamped.

    Return SVGT_NO_ERROR if the operation was completed successfully,
    else an error code.
*/
SVGTErrorCode svgtClearColor(SVGTfloat r,
                             SVGTfloat g,
                             SVGTfloat b,
                             SVGTfloat a);
```

```c
/*
    Specify if the whole drawing surface must be cleared by the
    svgtDocDraw function, before to draw the SVG document.

    Return SVGT_NO_ERROR if the operation was completed successfully,
    else an error code.
*/
SVGTErrorCode svgtClearPerform(SVGTboolean doClear);
```

For example, suppose that we want to clear the drawing surface with a dark blue color, then draw over it a transparent background representing mountains and clouds (`background.svg`), and finally put on top of it a user interface (`ui.svg`). The code would be the following:

```c
/* load SVG files */
SVGTHandle background = loadSvg("background.svg");
SVGTHandle ui = loadSvg("ui.svg");
/* create a drawing surface */
SVGTHandle surface = svgtSurfaceCreate(1024, 512);
/* set a dark blue clear color and enable surface clearing */
svgtClearColor(0.06f, 0.02f, 0.75f, 1.0f);
svgtClearPerform(SVGT_TRUE);
/* draw the background (NB: before the drawing, the surface
will be cleared with the dark blue color) */
svgtDocDraw(background, surface, SVGT_RENDERING_QUALITY_BETTER);
/* disable surface clearing and draw user interface */
svgtClearPerform(SVGT_FALSE);
svgtDocDraw(ui, surface, SVGT_RENDERING_QUALITY_BETTER);
```

Sometime it would be useful to select just a sub-region of the whole drawing surface for the rendering, enabling in this way the possibility to draw more than one SVG document within the same surface. It is possible to select the current drawing sub-region using the `svgtSurfaceViewportSet` function:

```c
/*
    Set destination viewport (i.e. a drawing surface rectangular area)
    where to map the source document viewport.

    The 'viewport' parameter must be an array of (at least)
    4 float entries, it must contain:
    
    - viewport[0] = top/left x
    - viewport[1] = top/left y
    - viewport[2] = width
    - viewport[3] = height

    The combined use of svgtDocViewportSet and svgtSurfaceViewportSet induces a
    transformation matrix, that will be used to draw the whole SVG document.
    The induced matrix grants that the document viewport is mapped onto the
    surface viewport (respecting the specified alignment): all SVG content will
    be drawn accordingly.

    NB: floating-point values of NaN are treated as 0, values of
    +Infinity and -Infinity are clamped to the largest and smallest
    available float values.

    This function returns:
    
    - SVGT_BAD_HANDLE_ERROR if specified surface handle is not valid

    - SVGT_ILLEGAL_ARGUMENT_ERROR if 'viewport' pointer is NULL
      or if it's not properly aligned

    - SVGT_ILLEGAL_ARGUMENT_ERROR if specified viewport width or
      height are less than or equal zero

    - SVGT_NO_ERROR if the operation was completed successfully
*/
SVGTErrorCode svgtSurfaceViewportSet(SVGTHandle surface,
                                     SVGTfloat* viewport);
```

When a drawing surface is created through the `svgtSurfaceCreate` function, its current target viewport is set as the entire boundary. In any case, the current viewport relative to a given surface can be retrieved using the following function:

```c
/*
    Get current destination viewport (i.e. a drawing surface rectangular area)
    where to map the source document viewport.

    The 'viewport' parameter must be an array of (at least)
    4 float entries, it will be filled with:
    - viewport[0] = top/left x
    - viewport[1] = top/left y
    - viewport[2] = width
    - viewport[3] = height

    This function returns:
    - SVGT_BAD_HANDLE_ERROR if specified surface handle is not valid
    
    - SVGT_ILLEGAL_ARGUMENT_ERROR if 'viewport' pointer is NULL
      or if it's not properly aligned
    
    - SVGT_NO_ERROR if the operation was completed successfully
*/
SVGTErrorCode svgtSurfaceViewportGet(SVGTHandle surface,
                                     SVGTfloat* viewport);
```

Now suppose that we want to draw four different SVG documents within the same drawing surface (of course over 4 different non-overlapping sub-regions), the code would look as follow:

```c
SVGTfloat srfViewport[4];
/* load SVG files */
SVGTHandle svg1 = loadSvg("car_coolant.svg");
SVGTHandle svg2 = loadSvg("car_maintenance.svg");
SVGTHandle svg3 = loadSvg("car_brake.svg");
SVGTHandle svg4 = loadSvg("car_oil.svg");
/* create a drawing surface */
SVGTHandle surface = svgtSurfaceCreate(512, 512);
/* set an opaque white clear color and enable surface clearing */
svgtClearColor(1.0f, 1.0f, 1.0f, 1.0f);
svgtClearPerform(SVGT_TRUE);
/* select an upper-left sub-region and draw the first SVG document */
srfViewport[0] = 0;
srfViewport[1] = 0;
srfViewport[2] = 256;
srfViewport[3] = 256;
svgtSurfaceViewportSet(surface, srfViewport);
svgtDocDraw(svg1, surface, SVGT_RENDERING_QUALITY_BETTER);
/* disable surface clearing */
svgtClearPerform(SVGT_FALSE);
/* select an upper-right sub-region and draw the second SVG document */
srfViewport[0] = 256;
srfViewport[1] = 0;
svgtSurfaceViewportSet(surface, srfViewport);
svgtDocDraw(svg2, surface, SVGT_RENDERING_QUALITY_BETTER);
/* select the whole lower-left sub-region and draw the third SVG document */
srfViewport[0] = 0;
srfViewport[1] = 256;
svgtSurfaceViewportSet(surface, srfViewport);
svgtDocDraw(svg3, surface, SVGT_RENDERING_QUALITY_BETTER);
/* select the whole lower-right sub-region and draw the fourth SVG document */
srfViewport[0] = 256;
srfViewport[1] = 256;
svgtSurfaceViewportSet(surface, srfViewport);
svgtDocDraw(svg4, surface, SVGT_RENDERING_QUALITY_BETTER);
```

| ![Surface Viewport ]({{site.url}}/assets/images/srf_viewport.png) | 
| :---: |
| *Usage of surface viewport* |

As for drawing surface, a viewport can be defined even for SVG documents. When an SVG document has been created through the `svgtDocCreate` function, its initial viewport is set equal to the viewBox XML attribute of the outermost `<svg>` element. SVG document viewport can be retrieved and set using the following functions:

```c
/*
    Get the document (logical) viewport to map onto the destination (drawing surface)
    viewport. When an SVG document has been created through the svgtDocCreate function,
    the initial value of its viewport is equal to the 'viewBox' attribute present in the
    outermost <svg> element.
    If such element does not contain the viewBox attribute, SVGT_NO_ERROR is returned and
    viewport array will be filled with zeros.

    The 'viewport' parameter must be an array of (at least) 4 float entries,
    it will be filled with:
    
    - viewport[0] = top/left x
    - viewport[1] = top/left y
    - viewport[2] = width
    - viewport[3] = height

    This function returns:
    - SVGT_BAD_HANDLE_ERROR if specified document handle is not valid
    
    - SVGT_ILLEGAL_ARGUMENT_ERROR if 'viewport' pointer is NULL or
      if it's not properly aligned
    
    - SVGT_NO_ERROR if the operation was completed successfully
*/
SVGTErrorCode svgtDocViewportGet(SVGTHandle svgDoc,
                                 SVGTfloat* viewport);
```

```c
/*
    Set the document (logical) viewport to map onto the destination
    (drawing surface) viewport.

    The 'viewport' parameter must be an array of (at least)
    4 float entries, it must contain:
    
    - viewport[0] = top/left x
    - viewport[1] = top/left y
    - viewport[2] = width
    - viewport[3] = height

    NB: floating-point values of NaN are treated as 0, values of +Inf and -Inf
    are clamped to the largest and smallest available float values.

    This function returns:
    - SVGT_BAD_HANDLE_ERROR if specified document handle is not valid
    
    - SVGT_ILLEGAL_ARGUMENT_ERROR if 'viewport' pointer is NULL or
      if it's not properly aligned
    
    - SVGT_ILLEGAL_ARGUMENT_ERROR if specified viewport width or
      height are less than or equal zero
    
    - SVGT_NO_ERROR if the operation was completed successfully
*/
SVGTErrorCode svgtDocViewportSet(SVGTHandle svgDoc,
                                 SVGTfloat* viewport);
```

The document viewport could be though as the source (or logical) viewport, the surface viewport instead could be though as the destination (or physical) viewport. The combined use of `svgtDocViewportSet` and `svgtSurfaceViewportSet` induces a transformation matrix, that will be used (by the `svgtDocDraw` function) to draw the whole SVG document. The induced matrix will ensure that the document viewport will be mapped onto the surface viewport: all SVG content will be drawn accordingly.

If the two viewports do not have the same aspect ratio (i.e. width-to-height ratio), we need to specify how the svgtDocDraw function is to display the SVG document. We do so using the following function:

```c
/*
    Set the document alignment.
    The alignment parameter indicates whether to force uniform scaling and, if so, the
    alignment method to use in case the aspect ratio of the document viewport doesn't
    match the aspect ratio of the surface viewport.

    The 'values' parameter must be an array of (at least)
    2 unsigned integers entries, it must contain:
    
    - values[0] = alignment (see SVGTAspectRatioAlign)
    - values[1] = meetOrSlice (see SVGTAspectRatioMeetOrSlice)

    This function returns:
    - SVGT_BAD_HANDLE_ERROR if specified document handle is not valid
    
    - SVGT_ILLEGAL_ARGUMENT_ERROR if 'viewport' pointer is NULL or
      if it's not properly aligned
    
    - SVGT_ILLEGAL_ARGUMENT_ERROR if specified alignment is not
      a valid SVGTAspectRatioAlign value
    
    - SVGT_ILLEGAL_ARGUMENT_ERROR if specified meetOrSlice is not
      a valid SVGTAspectRatioMeetOrSlice value
    
    - SVGT_NO_ERROR if the operation was completed successfully
*/
SVGTErrorCode svgtDocViewportAlignmentSet(SVGTHandle svgDoc,
                                          SVGTuint* values);
```

The function takes an array of two values as input (along with an SVG document of course): the first value tells how the document viewport is aligned within the surface viewport, the second value tells how the aspect ratio is to be preserved (if at all).

Alignment is defined by the `SVGTAspectRatioAlign` enumeration type:

```c
/*
    Alignment indicates whether to force uniform scaling and, if so,
    the alignment method to use in case the aspect ratio of the source
    (document) viewport doesn't match the aspect ratio of the destination
    (drawing surface) viewport.
*/
typedef enum {
    /*
        Do not force uniform scaling.
        Scale the graphic content of the given element non-uniformly if necessary such
        that the element's bounding box exactly matches the viewport rectangle.
        NB: in this case, the <meetOrSlice> value is ignored.
    */
    SVGT_ASPECT_RATIO_ALIGN_NONE,
    /*
        Force uniform scaling.
        Align the <min-x> of the source viewport with the smallest x value of
        the destination viewport.
        Align the <min-y> of the source viewport with the smallest y value of
        the destination viewport.
    */
    SVGT_ASPECT_RATIO_ALIGN_XMINYMIN,
    /*
        Force uniform scaling.
        Align the <mid-x> of the source viewport with the midpoint x value of
        the destination viewport.
        Align the <min-y> of the source viewport with the smallest y value of
        the destination viewport.
    */
    SVGT_ASPECT_RATIO_ALIGN_XMIDYMIN,
    /*
        Force uniform scaling.
        Align the <max-x> of the source viewport with the maximum x value of
        the destination viewport.
        Align the <min-y> of the source viewport with the smallest y value of
        the destination viewport.
    */
    SVGT_ASPECT_RATIO_ALIGN_XMAXYMIN,
    /*
        Force uniform scaling.
        Align the <min-x> of the source viewport with the smallest x value of
        the destination viewport.
        Align the <mid-y> of the source viewport with the midpoint y value of
        the destination viewport.
    */
    SVGT_ASPECT_RATIO_ALIGN_XMINYMID,
    /*
        Force uniform scaling.
        Align the <mid-x> of the source viewport with the midpoint x value of
        the destination viewport.
        Align the <mid-y> of the source viewport with the midpoint y value of
        the destination viewport.
    */
    SVGT_ASPECT_RATIO_ALIGN_XMIDYMID,
    /*
        Force uniform scaling.
        Align the <max-x> of the source viewport with the maximum x value of
        the destination viewport.
        Align the <mid-y> of the source viewport with the midpoint y value of
        the destination viewport.
    */
    SVGT_ASPECT_RATIO_ALIGN_XMAXYMID,
    /*
        Force uniform scaling.
        Align the <min-x> of the source viewport with the smallest x value of
        the destination viewport.
        Align the <max-y> of the source viewport with the maximum y value of
        the destination viewport.
    */
    SVGT_ASPECT_RATIO_ALIGN_XMINYMAX,
    /*
        Force uniform scaling.
        Align the <mid-x> of the source viewport with the midpoint x value of
        the destination viewport.
        Align the <max-y> of the source viewport with the maximum y value of
        the destination viewport.
    */
    SVGT_ASPECT_RATIO_ALIGN_XMIDYMAX,
    /*
        Force uniform scaling.
        Align the <max-x> of the source viewport with the maximum x value of
        the destination viewport.
        Align the <max-y> of the source viewport with the maximum y value of
        the destination viewport.
        */
        SVGT_ASPECT_RATIO_ALIGN_XMAXYMAX
} SVGTAspectRatioAlign;
```

The way in which the aspect ratio is to be preserved is defined by the `SVGTAspectRatioMeetOrSlice` enumeration type:

```c
typedef enum {
    /*
        Scale the graphic such that:
        
        - aspect ratio is preserved
        
        - the entire viewBox is visible within the viewport
        
        - the viewBox is scaled up as much as possible,
          while still meeting the other criteria
        
        In this case, if the aspect ratio of the graphic does not match the viewport,
        some of the viewport will extend beyond the bounds of the viewBox (i.e., the 
        area into which the viewBox will draw will be smaller than the viewport).
    */
    SVGT_ASPECT_RATIO_MEET,
    /*
        Scale the graphic such that:
        
        - aspect ratio is preserved
        
        - the entire viewport is covered by the viewBox
        
        - the viewBox is scaled down as much as possible,
          while still meeting the other criteria
        
        In this case, if the aspect ratio of the viewBox does not match the viewport,
        some of the viewBox will extend beyond the bounds of the viewport (i.e., the 
        area into which the viewBox will draw is larger than the viewport).
    */
    SVGT_ASPECT_RATIO_SLICE
} SVGTAspectRatioMeetOrSlice;
```

---