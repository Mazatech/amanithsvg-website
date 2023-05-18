---
layout: default
title: "Java"
date: 2018-01-01 08:00:00 +0100
chapter: 2
categories: [binding]
---

# Java binding

The Java binding of AmanithSVG API consists in two different layers:

 - a low level layer, that uses the [Java Native Interface mechanism](https://en.wikipedia.org/wiki/Java_Native_Interface) (also known as JNI): JNI allows Java code to call functions that are implemented within the AmanithSVG C/C++ .dll (Windows), .dylib (MacOS), .so (Linux). The [low level API]({{site.url}}/docs/api/001-data-types.html) is exposed through the [static class AmanithSVG](https://github.com/Mazatech/amanithsvg-sdk/blob/master/examples/svgViewer/platform/android/app/src/main/java/com/mazatech/svgt/AmanithSVG.java).

 - a high level layer that abstracts some details of the low level API

Here's a list of classes exposed by the high level Java layer:

 - [SVGTError](#svgterror)
 - [SVGTAlign](#svgtalign)
 - [SVGTMeetOrSlice](#svgtmeetorslice)
 - [SVGTRenderingQuality](#svgtrenderingquality)
 - [SVGTLogLevel](#svgtloglevel)
 - [SVGTResourceType](#svgtresourcetype)
 - [SVGTResourceHint](#svgtresourcehint)
 - [SVGColor](#svgcolor)
 - [SVGPoint](#svgpoint)
 - [SVGViewport](#svgviewport)
 - [SVGAlignment](#svgalignment)
 - [SVGDocument](#svgdocument)
 - [SVGSurface](#svgsurface)
 - [SVGPacker](#svgpacker)
 - [SVGAssetsConfig](#svgassetsconfig)
 - [SVGResource](#svgresource)
 - [SVGAssets](#svgassets)

---

## SVGTError

The [SVGTError](https://github.com/Mazatech/amanithsvg-sdk/blob/master/examples/svgViewer/platform/android/app/src/main/java/com/mazatech/svgt/SVGTError.java) enumeration wraps the native [SVGTErrorCode]({{site.url}}/docs/api/002-errors.html) enum type, and it represents all possible error codes signaled dy AmanithSVG functions.

---

## SVGTAlign

The [SVGTAlign](https://github.com/Mazatech/amanithsvg-sdk/blob/master/examples/svgViewer/platform/android/app/src/main/java/com/mazatech/svgt/SVGTAlign.java) enumeration wraps the native `SVGTAspectRatioAlign` enum type, and it defines the alignment method to use in case the aspect ratio of the source (document) viewport doesn't match the aspect ratio of the destination (drawing surface) viewport.

---

## SVGTMeetOrSlice

The [SVGTMeetOrSlice](https://github.com/Mazatech/amanithsvg-sdk/blob/master/examples/svgViewer/platform/android/app/src/main/java/com/mazatech/svgt/SVGTMeetOrSlice.java) enumeration wraps the native `SVGTAspectRatioMeetOrSlice` enum type, and it defines the scaling method to use in case the aspect ratio of the source (document) viewport doesn't match the aspect ratio of the destination (drawing surface) viewport.

---

## SVGTRenderingQuality

The [SVGTRenderingQuality](https://github.com/Mazatech/amanithsvg-sdk/blob/master/examples/svgViewer/platform/android/app/src/main/java/com/mazatech/svgt/SVGTRenderingQuality.java) enumeration wraps the native `SVGTRenderingQuality` enum type, and it defines the quality (and speed) of SVG drawings.

---

## SVGTLogLevel

The [SVGTLogLevel](https://github.com/Mazatech/amanithsvg-sdk/blob/master/examples/svgViewer/platform/android/app/src/main/java/com/mazatech/svgt/SVGTLogLevel.java) enumeration wraps the native `SVGTLogLevel` enum type, and it defines the possible logging levels (e.g. errors, warnings or just informational messages).

---

## SVGTResourceType

The [SVGTResourceType](https://github.com/Mazatech/amanithsvg-sdk/blob/master/examples/svgViewer/platform/android/app/src/main/java/com/mazatech/svgt/SVGTResourceType.java) enumeration wraps the native `SVGTResourceType` enum type, and it defines the type of each external resource provided to AmanithSVG.

---

## SVGTResourceHint

The [SVGTResourceHint](https://github.com/Mazatech/amanithsvg-sdk/blob/master/examples/svgViewer/platform/android/app/src/main/java/com/mazatech/svgt/SVGTResourceHint.java) enumeration wraps the native `SVGTResourceHint` enum type, and it defines all the possible hints for tagging external resources provided to AmanithSVG.

---

## SVGColor

[SVGColor](https://github.com/Mazatech/amanithsvg-sdk/blob/master/examples/svgViewer/platform/android/app/src/main/java/com/mazatech/svgt/SVGColor.java) is a simple class that exposes four (float) color components (red, green, blue, alpha), within the `[0.0f; 1.0f]` range. The class is so simple that it does not require further explanation.

---

## SVGPoint

[SVGPoint](https://github.com/Mazatech/amanithsvg-sdk/blob/master/examples/svgViewer/platform/android/app/src/main/java/com/mazatech/svgt/SVGPoint.java) is a simple class representing a 2D point, the class exposes two (float) properties: abscissa (x) and ordinate (y). The class is so simple that it does not require further explanation.

---

## SVGViewport

[SVGViewport](https://github.com/Mazatech/amanithsvg-sdk/blob/master/examples/svgViewer/platform/android/app/src/main/java/com/mazatech/svgt/SVGViewport.java) represents a rectangular area, specified by its top/left corner, a width and an height. The positive x-axis points towards the right, the positive y-axis points down.
The class exposes four (float) properties: x, y, width, height. The class is so simple that it does not require further explanation.

---

## SVGAlignment

The [SVGAlignment](https://github.com/Mazatech/amanithsvg-sdk/blob/master/examples/svgViewer/platform/android/app/src/main/java/com/mazatech/svgt/SVGAlignment.java) class represents a couple of values, one taken from the `SVGTAlign` enum type and the other taken from the `SVGTMeetOrSlice` enum type.
Alignment indicates whether to force uniform scaling and, if so, the alignment method to use in case the aspect ratio of the source viewport doesn't match the aspect ratio of the destination viewport.
For detailed explanation of `SVGTAlign` and `SVGTMeetOrSlice` enum type, please refer to the [low level API documentation]({{site.url}}/docs/api/007-rendering-svg-document.html#documents-alignment).

| &nbsp; |
| :---: |
| *A brief visual example of possible alignments* |
{:.tbl_images .preserveAspectRatio}

---

## SVGDocument

In order to render SVG contents, AmanithSVG must create an optimized in-memory representation of SVG files: a such representation is called [SVG Document](https://github.com/Mazatech/amanithsvg-sdk/blob/master/examples/svgViewer/platform/android/app/src/main/java/com/mazatech/svgt/SVGDocument.java).
An SVG document can be created through `SVGAssets`'s `createDocument` function, specifying the xml text. The document will be parsed immediately, and the internal drawing tree will be created.
Once the document has been created, it can be drawn several times onto one (or more) drawing surface.
In order to draw a document, we must:

 1. create a drawing surface using `SVGAssets`'s `createSurface` function
 2. call `SVGSurface`'s `draw` method, specifying the document to draw

Example:

```java
SVGAssets svg = < instantiate AmanithSVG >;
String xml = new String(Files.readAllBytes(Paths.get("animals.svg")));
SVGDocument doc = svg.createDocument(xml);

< do something with document >

/* release AmanithSVG */
svg.dispose();
```

`SVGDocument` class exposes four main properties: handle, width, height, viewport, aspectRatio, accessible through their respective "get" methods `getHandle()`, `getWidth()`, `getHeight()`, `getViewport()`, `getAspectRatio()`. Handle, width and height are read-only values, all other properties can also be set through their respective "set" methods `setAspectRatio`, `setViewport`.

```java
/* 
    AmanithSVG document handle (read only); it's the handle created by the low level
    svgtDocCreate function.
*/
SVGTHandle getHandle();

/*
    SVG content itself optionally can provide information about the appropriate
    viewport region for the content via the 'width' and 'height' XML attributes
    on the outermost <svg> element.
    Use this method to get the suggested viewport width, in pixels.

    It returns a negative number (i.e. an invalid width) in the following cases:
    - outermost element is not an <svg> element
    - outermost <svg> element doesn't have a 'width' attribute specified
    - outermost <svg> element has a negative 'width' attribute specified
      (e.g. width="-50")
    - outermost <svg> element has a 'width' attribute specified in relative measure
      units (i.e. em, ex, % percentage)
*/
float getWidth();

/*
    SVG content itself optionally can provide information about the appropriate
    viewport region for the content via the 'width' and 'height' XML attributes
    on the outermost <svg> element.
    Use this method to get the suggested viewport height, in pixels.

    It returns a negative number (i.e. an invalid height) in the following cases:
    - outermost element is not an <svg> element
    - outermost <svg> element doesn't have a 'height' attribute specified
    - outermost <svg> element has a negative 'height' attribute specified
      (e.g. height="-30")
    - outermost <svg> element has a 'height' attribute specified in relative measure
      units (i.e. em, ex, % percentage)
*/
float getHeight();

/*
    The document (logical) viewport to map onto the destination (drawing surface)
    viewport.
    When an SVG document has been created through the SVGAssets.createDocument
    function, the initial value of its viewport is equal to the 'viewBox' attribute
    present in the outermost <svg> element.
*/
SVGViewport getViewport();

/*
    Viewport aspect ratio.
    The alignment parameter indicates whether to force uniform scaling and, if so,
    the alignment method to use in case the aspect ratio of the document viewport
    doesn't match the aspect ratio of the surface viewport.
*/
SVGAlignment getAspectRatio();

/* Set a new alignment for the document viewport. */
void setAspectRatio(SVGAlignment aspectRatio);

/* Set a new viewport for the document. */
void setViewport(SVGViewport viewport);
```

---

## SVGSurface

[SVGSurface](https://github.com/Mazatech/amanithsvg-sdk/blob/master/examples/svgViewer/platform/android/app/src/main/java/com/mazatech/svgt/SVGSurface.java) class represents a drawing surface.

A drawing surface is just a rectangular area made of pixels, where each pixel is represented internally by a 32bit unsigned integer.
A pixel is made of four 8-bit components: red, green, blue, alpha. Coordinate system is the same of SVG specifications: top/left pixel has coordinate (0, 0), with the positive x-axis pointing towards the right and the positive y-axis pointing down.

An `SVGSurface` can be created by using the `SVGAssets.createSurface` function, specifying its dimensions in pixels.

`SVGSurface` class exposes four main properties: handle, width, height, viewport, accessible through their respective "get" methods `getHandle()`, `getWidth()`, `getHeight()`, `getViewport()`.

```java
/*
    AmanithSVG surface handle (read only); it's the handle created by the low level
    svgtSurfaceCreate function.
*/
SVGTHandle getHandle();

/* Get current surface width, in pixels. */
int getWidth();

/* Get current surface height, in pixels. */
int getHeight();

/*
    The surface viewport (i.e. a drawing surface rectangular area), where to map
    the source document viewport.
    The combined use of surface and document viewport, induces a transformation
    matrix, that will be used to draw the whole SVG document. The induced matrix
    grants that the document viewport is mapped onto the surface viewport
    (respecting the specified alignment): all SVG content will be drawn accordingly.
*/
SVGViewport getViewport();
```

A drawing surface can be resized by calling the `resize` method:

```java
/*
    Resize the surface, specifying new dimensions in pixels; it returns
    SVGTError.None if the operation was completed successfully, else an error code.
    After resizing, the surface viewport will be reset to the whole surface.
*/
SVGTError resize(int newWidth,
                 int newHeight);
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
SVGTError draw(SVGDocument document,
               SVGColor clearColor,
               SVGTRenderingQuality renderingQuality);
```

Here's a complete example:

```java
SVGAssets svg = < instantiate AmanithSVG >;
/* load an SVG file */
String xml = new String(Files.readAllBytes(Paths.get("animals.svg")));
SVGDocument doc = svg.createDocument(xml);

/* create a 512x512 drawing surface */
SVGSurface srf = svg.createSurface(512, 512);

/* draw the SVG */
srf.draw(doc, SVGColor.White, SVGTRenderingQuality.Better);
< do something with surface pixels (e.g. upload pixels to a GPU texture) >

/* destroy surface and document */
doc.dispose();
srf.dispose();
/* release AmanithSVG */
svg.dispose();
```

| &nbsp; |
| :---: |
| *Rendering result* |
{:.tbl_images .animalsSvg}

By modifying the drawing surface viewport, it's possible to draw more than one SVG document on it. For example, suppose that we want to draw four different SVG documents within the same drawing surface (of course over 4 different non-overlapping sub-regions), the code would look as follow:

```java
SVGAssets svg = < instantiate AmanithSVG >;
/* load SVG files */
SVGDocument doc1 = svg.createDocument(new String(Files.readAllBytes(Paths.get("car_coolant.svg"))));
SVGDocument doc2 = svg.createDocument(new String(Files.readAllBytes(Paths.get("car_maintenance.svg"))));
SVGDocument doc3 = svg.createDocument(new String(Files.readAllBytes(Paths.get("car_brake.svg"))));
SVGDocument doc4 = svg.createDocument(new String(Files.readAllBytes(Paths.get("car_oil.svg"))));

/* create a drawing surface */
SVGSurface srf = svg.createSurface(512, 512);

/* select an upper-left sub-region and draw the first SVG document */
srf.setViewport(new SVGViewport(0, 0, 256, 256));
srf.draw(doc1, SVGColor.White, SVGTRenderingQuality.Better);

/* select an upper-right sub-region and draw the second SVG document */
srf.setViewport(new SVGViewport(256, 0, 256, 256));
srf.draw(doc2, null, SVGTRenderingQuality.Better);

/* select the whole lower-left sub-region and draw the third SVG document */
srf.setViewport(new SVGViewport(0, 256, 256, 256));
srf.draw(doc3, null, SVGTRenderingQuality.Better);

/* select the whole lower-right sub-region and draw the fourth SVG document */
srf.setViewport(new SVGViewport(256, 256, 256, 256));
srf.draw(doc4, null, SVGTRenderingQuality.Better);

< do something with surface pixels (e.g. upload pixels to a GPU texture) >

/* destroy surface and documents */
doc1.dispose();
doc2.dispose();
doc3.dispose();
doc4.dispose();
srf.dispose();
/* release AmanithSVG */
svg.dispose();
```

| &nbsp; |
| :---: |
| *Usage of surface viewport* |
{:.tbl_images .srf_viewport}

---

## SVGPacker

Besides rendering single SVG documents over drawing surfaces, AmanithSVG can pack the rendering of one or more SVG documents within one or more drawing surfaces, automatically. For each given SVG, you can choose to pack the whole document or instead to pack each first-level element separately.

The whole process consists of some steps that must be followed in this order:

 - create an instance of [SVGPacker](https://github.com/Mazatech/amanithsvg-sdk/blob/master/examples/svgViewer/platform/android/app/src/main/java/com/mazatech/svgt/SVGPacker.java) class
 - start the packing process using the `SVGPacker.begin` method
 - add one or more SVG document to the process, using the `SVGPacker.add` method
 - stop the packing process using the `SVGPacker.end` method
 - for each atlas information (`SVGPacker.SVGPackedBin`) returned by the `SVGPacker.end` method:
   - create a drawing surface with the same atlas dimensions, using the `SVGAssets.createSurface` function
   - draw the SVG documents/elements packed in the current atlas (`SVGPacker.SVGPackedBin`), using the `SVGSurface.draw` method

Here is a complete example code:

```java
SVGAssets svg = < instantiate AmanithSVG >;

/* create an SVG document */
SVGDocument doc = svg.createDocument(new String(Files.readAllBytes(Paths.get("orc.svg"))));

/* initialize the packing process: generate atlases with a maximum 512 pixels dimension and no additional scale */
SVGPacker packer = svg.createPacker(1.0f, 512, 1, false);

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
            SVGSurface srf = svg.createSurface(bin.Width, bin.Height);
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
/* release AmanithSVG */
svg.dispose();
```

If we run the example code using this [orc.svg file](https://github.com/Mazatech/amanithsvg-sdk/blob/master/examples/unity/Assets/SVGAssets/SVGFiles/orc.svg.txt), it will produce a single `452 x 108` bin, with the following packed rectangles:

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
| *orc.svg* |
{:.tbl_images .orcSvg}

| &nbsp; |
| :---: |
| *orc atlas* |
{:.tbl_images .orcSvgAtlas}

It is important to note that the whole packing process, as well as the `SVGSurface.draw(SVGPacker.SVGPackedBin, SVGColor, SVGTRenderingQuality)` method, is not affected by SVG documents viewports nor by drawing surfaces viewports. In other words, the viewport values set through `SVGDocument.setViewport` and `SVGSurface.setViewport` methods do not affect the behavior of packing processes and atlas generation.

For a more detailed explanation on the packing process, please refer to the relative [low level api]({{site.url}}/docs/api/008-packing-atlas-generation.html).

---

## SVGResource

This class represents an external (binary) resource, available for rendering SVG documents. Each resource is defined by a unique string identifier (typically the file name, without the path), a type (font or image), and a set of hints; all such properties must be specified in `SVGResource` constructor. `SVGResource` class cannot be instantiated directly, because it has an abstract `getStream` method that must be implemented according to the target platform (or development tool/environment): such method must return an in-memory representation of the underlying binary TTF / OTF / WOFF / JPEG / PNG file.

```java
/* Constructor. */
abstract SVGResource(String id,
                     SVGTResourceType type,
                     final EnumSet<SVGTResourceHint> hints);

/* Get input/read stream for the resource. */
abstract InputStream getStream();

/* Resource identifier. */
String _id;

/* Resource type. */
SVGTResourceType _type;

/* Resource hints. */
EnumSet<SVGTResourceHint> _hints;
```

Resources are provided to `SVGAssets` through an instance of a class derived from `SVGAssetsConfig`.

---

## SVGAssetsConfig

The [SVGAssetsConfig](https://github.com/Mazatech/amanithsvg-sdk/blob/master/examples/svgViewer/platform/android/app/src/main/java/com/mazatech/svgt/SVGAssetsConfig.java) class provides configuration parameters, along with font resources, to an `SVGAssets` instance. Configuration parameters can be divided in two sub-sets:

- general user-defined parameters: curves quality, user-agent language, log level
- platform-specific parameters: device screen with/height in pixels, screen dpi

The class is `abstract` so it cannot be instantiated directly; it must be extended by implementing the mechanism to access resource files (defined by the `SVGResource` class).
In particular, each class that extends `SVGAssetsConfig` must implement the `resourcesCount` and `getResource` methods. See details:


```java
/*
    Constructor, device screen properties must be supplied
    (with/height in pixels and dpi).
*/
SVGAssetsConfig(int screenWidth,
                int screenHeight,
                float screenDpi);

/* Get screen resolution width, in pixels. */
int getScreenWidth();

/* Get screen resolution height, in pixels. */
int getScreenHeight();

/* Get screen dpi. */
float getScreenDpi();

/*
    Get curves quality, used by AmanithSVG geometric kernel to approximate curves
    with straight line segments (flattening). Valid range is [1; 100], where 100
    represents the best quality.

    A zero or negative value means "keep the default one".
*/
float getCurvesQuality();

/*
    Set curves quality, used by AmanithSVG geometric kernel to approximate curves
    with straight line segments (flattening). Valid range is [1; 100], where 100
    represents the best quality.

    A zero or negative value means "keep the default one".
*/
void setCurvesQuality(float quality);

/* Get user-agent language. */
String getLanguage();

/*
    Set the user-agent language.

    Specify the system/user-agent language; this setting will affect the
    conditional rendering of <switch> elements and elements with
    'systemLanguage' attribute specified. The given argument must be a
    non-empty list of languages separated by semicolon
    (e.g. "en-US;en-GB;it;es")
*/
void setLanguage(String languages);

/* Get log level. */
EnumSet<SVGTLogLevel> getLogLevel();

/* Get log capacity, in characters. */
int getLogCapacity();

/*
    Set log parameters (log level and log capacity).

    If the specified log level is empty, logging is disabled.
    If the specified log capacity (in characters) is less than or equal
    zero, logging is disabled.
*/
void setLogParameters(EnumSet<SVGTLogLevel> logLevel,
                      int logCapacity);

/*
    Get the number of (external) resources provided by this configuration.
    It must be implemented by all derived classes.
*/
abstract int resourcesCount();

/*
    Get a resource given an index.
    If the given index is less than zero or greater or equal to the value
    returned by 'resourcesCount', a null resource is returned.

    It must be implemented by all derived classes.
*/
abstract SVGResource getResource(int index);
```

---

## SVGAssets

[SVGAssets](https://github.com/Mazatech/amanithsvg-sdk/blob/master/examples/svgViewer/platform/android/app/src/main/java/com/mazatech/svgt/SVGAssets.java) is the class through which you can create `SVGDocument`, `SVGSurface` and `SVGPacker` instances; here are the main methods:

```java
/*
    Create a drawing surface, specifying its dimensions in pixels.

    Supplied dimensions should be positive numbers greater than zero, else
    a null instance will be returned.
*/
SVGSurface createSurface(int width,
                         int height);

/*
    Create and load an SVG document, specifying the whole XML string.
    If supplied XML string is null or empty, a null instance will be returned.
*/
SVGDocument createDocument(String xmlText);

/*
    Create an SVG packer, specifying a scale factor.
    
    Every collected SVG document/element will be packed into rectangular bins,
    whose dimensions won't exceed the specified 'maxTexturesDimension' in pixels.

    If true, 'pow2Textures' will force bins to have power-of-two dimensions.
    Each rectangle will be separated from the others by the specified 'border' in pixels.

    The specified 'scale' factor will be applied to all collected SVG documents/elements,
    in order to realize resolution-independent atlases.
*/
SVGPacker createPacker(float scale,
                       int maxTexturesDimension,
                       int border,
                       boolean pow2Textures);

/* Get library version. */
static String getVersion();
```

AmanithSVG can provide detailed information about error situations that can occur when parsing and rendering SVG content. For example AmanithSVG can report if a closing tag did not match the opening one (or if some tag was not closed at all) when parsing an SVG document; or if a font needed for rendering has not been set. In case of error, it is possible to receive additional information on the conditions (which caused the error) in the form of text messages that AmanithSVG can append to a given memory buffer.
Log level and the capacity (in characters) of the internal log buffer can be configured through the `SVGAssetsConfig` class, provided to `SVGAssets` initialization mechanism.

```java
/* Get AmanithSVG log content as a string. */
String getLog();

/*
    Clear the AmanithSVG log buffer set for the current thread.

    If specified, make sure AmanithSVG no longer uses a log buffer for
    the current thread. In this case, logging can be reactivated (for
    the calling thread) by calling this method with a 'false' parameter.
*/
SVGTError logClear(boolean stopLogging);
```

From an application point of view, it could be useful to apppend some custom messages to the AmanithSVG log buffer. One of the main purposes can be to insert debug messages, to better understand the interactions between the application and the AmanithSVG library. In this way the application can keep track of all SVG-related activities, and the whole log buffer can be analyzed later (if needed).

```java
/*
    Append an informational message to the AmanithSVG log buffer
    set for the current thread.
*/
boolean logInfo(String message);

/*
    Append a warning message to the AmanithSVG log buffer
    set for the current thread.
*/
boolean logWarning(String message);

/*
    Append an error message to the AmanithSVG log buffer
    set for the current thread.
*/
boolean logError(String message);
```

Here's an example:

```java
SVGAssets svg = < instantiate AmanithSVG >;

/* clear AmanithSVG log */
svg.logClear(false);

< load SVG, draw it >

/* print AmanithSVG log content, in order to get details on possible warnings/errors */
System.out.println(svg.getLog());

/* make sure AmanithSVG no longer uses a log buffer (i.e. disable logging) */
svg.logClear(true);

/* release AmanithSVG */
svg.dispose();
```

---
