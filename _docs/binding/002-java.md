---
layout: default
title: "Java"
date: 2018-01-01 08:00:00 +0100
chapter: 2
categories: [binding]
---

# Java binding

The Java binding of AmanithSVG API consists in two different layers:

 - a low level layer, that uses the [Java Native Interface mechanism](https://en.wikipedia.org/wiki/Java_Native_Interface) (also known as JNI): JNI allows Java code to call functions that are implemented within the AmanithSVG C/C++ .dll (Windows), .dylib (MacOS), .so (Linux). The [low level API]({{site.url}}/docs/api/001-data-types.html) is exposed through the [static class AmanithSVG](http://github.com/Mazatech/amanithsvg-bindings/blob/master/Java/com/mazatech/svgt/AmanithSVG.java).

 - a high level layer that abstracts some details of the low level API

Here's a list of classes exposed by the high level Java layer:

 - [SVGColor](#svgcolor)
 - [SVGPoint](#svgpoint)
 - [SVGViewport](#svgviewport)
 - [SVGAlignment](#svgalignment)
 - [SVGDocument](#svgdocument)
 - [SVGSurface](#svgsurface)
 - [SVGPacker](#svgpacker)
 - [SVGAssets](#svgassets)


---

## SVGColor

`SVGColor` is a simple class that exposes four (float) color components (red, green, blue, alpha), within the `[0.0f; 1.0f]` range. The class is so simple that it does not require further explanation.


---

## SVGPoint

`SVGPoint` is a simple class representing a 2D point, the class exposes two (float) properties: abscissa (x) and ordinate (y). The class is so simple that it does not require further explanation.


---

## SVGViewport

A viewport represents a rectangular area, specified by its top/left corner, a width and an height. The positive x-axis points towards the right, the positive y-axis points down.
The class exposes four (float) properties: x, y, width, height. The class is so simple that it does not require further explanation.


---

## SVGAlignment

This class represents a couple of values, one taken from the `SVGTAlign` enum type and the other taken from the `SVGTMeetOrSlice` enum type.
Alignment indicates whether to force uniform scaling and, if so, the alignment method to use in case the aspect ratio of the source viewport doesn't match the aspect ratio of the destination viewport.
For detailed explanation of `SVGTAlign` and `SVGTMeetOrSlice` enum type, please refer to the [low level API documentation]({{site.url}}/docs/api/007-rendering-svg-document.html#documents-alignment).

| ![Alignments](http://www.w3.org/TR/SVG11/images/coords/PreserveAspectRatio.png) | 
| :---: |
| *A brief visual example of possible alignments* |


---

## SVGDocument

In order to render SVG contents, AmanithSVG must create an optimized in-memory representation of SVG files: a such representation is called an SVG document.
An SVG document can be created through `SVGAssets.CreateDocument` function, specifying the xml text. The document will be parsed immediately, and the internal drawing tree will be created.
Once the document has been created, it can be drawn several times onto one (or more) drawing surface.
In order to draw a document, we must:

 1. create a drawing surface using `SVGAssets.createSurface`
 1. call surface `draw` method, specifying the document to draw

Example:

```java
String xml = new String(Files.readAllBytes(Paths.get("animals.svg")));
SVGDocument doc = SVGAssets.CreateDocument(xml);
```

`SVGDocument` class exposes four main properties: handle, width, height, viewport, aspectRatio, accessible through their respective "get" methods `getHandle()`, `getWidth()`, `getHeight()`, `getViewport()`, `getAspectRatio()`.

```java
/* 
    AmanithSVG document handle (read only); it's the handle created by the low level
    svgtDocCreate function.
*/
public SVGTHandle getHandle();

/*
    SVG content itself optionally can provide information about the appropriate
    viewport region for the content via the 'width' and 'height' XML attributes
    on the outermost <svg> element.
    Use this method to get the suggested viewport width, in pixels.

    It returns -1 (i.e. an invalid width) in the following cases:
    - outermost element is not an <svg> element
    - outermost <svg> element doesn't have a 'width' attribute specified
    - outermost <svg> element has a 'width' attribute specified in relative measure
      units (i.e. em, ex, % percentage)
*/
public float getWidth();

/*
    SVG content itself optionally can provide information about the appropriate
    viewport region for the content via the 'width' and 'height' XML attributes
    on the outermost <svg> element.
    Use this method to get the suggested viewport height, in pixels.

    It returns -1 (i.e. an invalid height) in the following cases:
    - outermost element is not an <svg> element
    - outermost <svg> element doesn't have a 'height' attribute specified
    - outermost <svg> element has a 'height' attribute specified in relative measure
      units (i.e. em, ex, % percentage)
*/
public float getHeight();

/*
    The document (logical) viewport to map onto the destination (drawing surface)
    viewport.
    When an SVG document has been created through the SVGAssets.CreateDocument
    function, the initial value of its viewport is equal to the 'viewBox' attribute
    present in the outermost <svg> element.
*/
public SVGViewport getViewport();

/*
    Viewport aspect ratio.
    The alignment parameter indicates whether to force uniform scaling and, if so,
    the alignment method to use in case the aspect ratio of the document viewport
    doesn't match the aspect ratio of the surface viewport.
*/
public SVGAlignment getAspectRatio();
```

---

## SVGSurface

`SVGSurface` class represents a drawing surface.

A drawing surface is just a rectangular area made of pixels, where each pixel is represented internally by a 32bit unsigned integer.
A pixel is made of four 8-bit components: red, green, blue, alpha. Coordinate system is the same of SVG specifications: top/left pixel has coordinate (0, 0), with the positive x-axis pointing towards the right and the positive y-axis pointing down.

An `SVGSurface` can be created by using the `SVGAssets.createSurface` function, specifying its dimensions in pixels.

`SVGSurface` class exposes four main properties: handle, width, height, viewport, accessible through their respective "get" methods `getHandle()`, `getWidth()`, `getHeight()`, `getViewport()`.

```java
/*
    AmanithSVG surface handle (read only); it's the handle created by the low level
    svgtSurfaceCreate function
*/
public SVGTHandle getHandle();

/* Get current surface width, in pixels. */
public int getWidth();

/* Get current surface height, in pixels. */
public int getHeight();

/*
    The surface viewport (i.e. a drawing surface rectangular area), where to map
    the source document viewport.
    The combined use of surface and document viewport, induces a transformation
    matrix, that will be used to draw the whole SVG document. The induced matrix
    grants that the document viewport is mapped onto the surface viewport
    (respecting the specified alignment): all SVG content will be drawn accordingly.
*/
public SVGViewport getViewport();
```

A drawing surface can be resized by calling the `resize` method:

```java
/*
    Resize the surface, specifying new dimensions in pixels; it returns
    SVGTError.None if the operation was completed successfully, else an error code.
    After resizing, the surface viewport will be reset to the whole surface.
*/
public SVGTError resize(int newWidth, int newHeight);
```

Once that an `SVGDocument` has been created, it can be rendered over a drawing surface using the following `SVGSurface` method:

```java
/*
    Draw an SVG document, on this drawing surface.
    First the drawing surface is cleared if a valid (i.e. not null) clear color
    is provided. Then the specified document, if valid, is drawn.
    It returns SVGTError.None if the operation was completed successfully, else
    an error code.
*/
public SVGTError draw(SVGDocument document,
                      SVGColor clearColor,
                      SVGTRenderingQuality renderingQuality);
```

Here's a complete example:

```java
/* load an SVG file */
String xml = new String(Files.readAllBytes(Paths.get("animals.svg")));
SVGDocument doc = SVGAssets.createDocument(xml);

/* create a 512x512 drawing surface */
SVGSurface srf = SVGAssets.createSurface(512, 512);

/* draw the SVG */
srf.draw(doc, SVGColor.White, SVGTRenderingQuality.Better);
< do something with surface pixels (e.g. upload pixels to a GPU texture) >

/* destroy surface and document */
doc.dispose();
srf.dispose();
```

| ![Animals]({{site.url}}/assets/images/animals.png) | 
| :---: |
| *Rendering result* |

By modifying the drawing surface viewport, it's possible to draw more than one SVG document on it. For example, suppose that we want to draw four different SVG documents within the same drawing surface (of course over 4 different non-overlapping sub-regions), the code would look as follow:

```java
/* load SVG files */
SVGDocument svg1 = SVGAssets.createDocument(new String(Files.readAllBytes(Paths.get("car_coolant.svg"))));
SVGDocument svg2 = SVGAssets.createDocument(new String(Files.readAllBytes(Paths.get("car_maintenance.svg"))));
SVGDocument svg3 = SVGAssets.createDocument(new String(Files.readAllBytes(Paths.get("car_brake.svg"))));
SVGDocument svg4 = SVGAssets.createDocument(new String(Files.readAllBytes(Paths.get("car_oil.svg"))));

/* create a drawing surface */
SVGSurface srf = SVGAssets.createSurface(512, 512);

/* select an upper-left sub-region and draw the first SVG document */
srf.setViewport(new SVGViewport(0, 0, 256, 256));
srf.draw(svg1, SVGColor.White, SVGTRenderingQuality.Better);

/* select an upper-right sub-region and draw the second SVG document */
srf.setViewport(new SVGViewport(256, 0, 256, 256));
srf.draw(svg2, null, SVGTRenderingQuality.Better);

/* select the whole lower-left sub-region and draw the third SVG document */
srf.setViewport(new SVGViewport(0, 256, 256, 256));
srf.draw(svg3, null, SVGTRenderingQuality.Better);

/* select the whole lower-right sub-region and draw the fourth SVG document */
srf.setViewport(new SVGViewport(256, 256, 256, 256));
srf.draw(svg4, null, SVGTRenderingQuality.Better);

< do something with surface pixels (e.g. upload pixels to a GPU texture) >

/* destroy surface and documents */
svg1.dispose();
svg2.dispose();
svg3.dispose();
svg4.dispose();
srf.dispose();
```

| ![Surface Viewport ]({{site.url}}/assets/images/srf_viewport.png) | 
| :---: |
| *Usage of surface viewport* |


---

## SVGPacker

Besides rendering single SVG documents over drawing surfaces, AmanithSVG can pack the rendering of one or more SVG documents within one or more drawing surfaces, automatically. For each given SVG, you can choose to pack the whole document or instead to pack each first-level element separately.

The whole process consists of some steps that must be followed in this order:

 - create an instance of `SVGPacker` class
 - start the packing process using the `SVGPacker.begin` method
 - add one or more SVG document to the process, using the `SVGPacker.add` method
 - stop the packing process using the `SVGPacker.end` method
 - for each atlas information (`SVGPacker.SVGPackedBin`) returned by the `SVGPacker.end` method:
   - create a drawing surface with the same atlas dimensions, using the `SVGAssets.createSurface` function
   - draw the SVG documents/elements packed in the current atlas (`SVGPacker.SVGPackedBin`), using the `SVGSurface.draw` method

Here is a complete example code:

```java
/* create an SVG document */
SVGDocument doc = SVGAssets.createDocument(new String(Files.readAllBytes(Paths.get("orc.svg"))));

/* initialize the packing process: generate atlases with a maximum 512 pixels dimension and no additional scale */
SVGPacker packer = SVGAssets.createPacker(1.0f, 512, 1, false);

if (packer.begin()) {
    
    int[] info = new int[2];
    // add the document to the packer, and get back the actual number of packed bounding boxes
    packer.add(doc, true, 1.0f, info);

    // info[0] = number of collected bounding boxes
    // info[1] = the actual number of packed bounding boxes
    if ((info == null) || (info[1] != info[0])) {
        System.out.println("Some SVG elements cannot be packed!");
        System.out.println("Specified maximum texture dimensions do not allow to pack all SVG elements");
        // close the packing process without doing anything
        packer.end(false);
    }
    else {
        // finalize the packing process and get back generated bins
        SVGPacker.SVGPackedBin[] bins = packer.end(true);

        for (int i = 0; i < bins.length; ++i) {
            // extract the bin (i.e. get atlas width, height and the number of packed documents/elements)
            SVGPacker.SVGPackedBin bin = bins[i];
            /* create a drawing surface with the same atlas dimension */
            SVGSurface srf = SVGAssets.createSurface(bin.Width, bin.Height);
            /* draw all documents/elements over the drawing surface */
            srf.draw(bin, SVGColor.Clear, SVGTRenderingQuality.Better);
            /* if needed, process the drawing surface (e.g. upload its pixels on a GPU texture) and then destroy it */
            < do something with surface pixels (e.g. upload pixels to a GPU texture) >
            /* destroy the surface */
            srf.dispose();
        }
    }
}

/* destroy the document */
doc.dispose();
```

If we run the example code using this [orc.svg file](https://github.com/Mazatech/amanithsvg-bindings/blob/master/Unity/Assets/SVGAssets/SVGFiles/orc.svg.txt), it will produce a single 452 x 108 atlas, with the following packed rectangles:

|          | elemName | originalX | originalY | x    | y    | width | height | zOrder |
| :------- | :------- | --------: | --------: | ---: | ---: | ----: | -----: | -----: |
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
{:.rwd-table2}

| ![Orc svg]({{site.url}}/assets/images/orc.png) | 
| :---: |
| *orc.svg* |

| ![Orc atlas]({{site.url}}/assets/images/orc_atlas.png) | 
| :---: |
| *orc atlas* |

It is important to note that the whole packing process, as well as the `SVGSurface.draw(SVGPacker.SVGPackedBin, SVGColor, SVGTRenderingQuality)` method, is not affected by SVG documents viewports nor by drawing surfaces viewports. In other words, the viewport values set through `SVGDocument.setViewport` and `SVGSurface.setViewport` methods do not affect the behavior of packing processes and atlas generation.

For a more detailed explanation on the packing process, please refer to the relative [low level api]({{site.url}}/docs/api/008-packing-atlas-generation.html).


---

## SVGAssets

`SVGAssets` is a static class through which you can create `SVGDocument`, `SVGSurface` and `SVGPacker` instances; here are the main methods:

```java
/* Create a drawing surface, specifying its dimensions in pixels. */
public static SVGSurface createSurface(int width, int height);

/* Create and load an SVG document, specifying the whole xml string. */
public static SVGDocument createDocument(String xmlText);

/*
    Create an SVG packer, specifying a scale factor.
    Every collected SVG document/element will be packed into rectangular bins,
    whose dimensions won't exceed the specified 'maxTexturesDimension' in pixels.
    If true, 'pow2Textures' will force bins to have power-of-two dimensions.
    Each rectangle will be separated from the others by the specified 'border' in pixels.
    The specified 'scale' factor will be applied to all collected SVG documents/elements,
    in order to realize resolution-independent atlases.
*/
public static SVGPacker createPacker(float scale,
                                     int maxTexturesDimension,
                                     int border,
                                     boolean pow2Textures);
```

There are also three main methods that **must** be implemented according to the target windowing/graphics system. The correct implementation of such three methods, allows AmanithSVG to resolve SVG lengths expressed in percentage.

```java
/* Get screen/device horizontal resolution, in pixels. */
public static int getScreenResolutionWidth();

/* Get screen/device vertical resolution, in pixels. */
public static int getScreenResolutionHeight();

/* Get screen/device dpi (dots per inch). */
public static float getScreenDpi();
```

Because there exists several Java based windowing/graphics environments, the implementation of such three methods is left to the developer.
The default implementation uses [libGDX](http://libgdx.badlogicgames.com) as main backend, for the purpose of being able to use the Java binding with a such famous crossplatform library; if you're not using libGDX, it is highly recommended that you implement immediately the `getScreenResolutionWidth`, `getScreenResolutionHeight` and `getScreenDpi` methods according to your windowing/graphics environment API.

NB: before using any other method of the `SVGAssets` class (e.g. `createSurface`, `createDocument`, `createPacker`), it is **mandatory** to call its `init()` funtion as a first thing to do:

```java
/* Initialize the AmanithSVG library as well as the JNI wrapper mechanism. */
public static void init();
```

For a complete Java example, please have a look at [this](http://github.com/Mazatech/amanithsvg-bindings/tree/master/libGDX/gameCards) (libGDX is used).

---
