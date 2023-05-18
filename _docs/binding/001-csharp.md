---
layout: default
title: "C#"
date: 2018-01-01 08:00:00 +0100
chapter: 1
categories: [binding]
---

# C# binding

The C# binding of AmanithSVG API consists in two different layers:

 - a low level layer, that uses the [Platform Invoke mechanism](https://en.wikipedia.org/wiki/Platform_Invocation_Services): PInvoke allows .NET code to call functions that are implemented within the AmanithSVG C/C++ .dll (Windows), .dylib (MacOS X), .so (Linux). It is important to understand that the main benefit of this approach is in the ability to call the same C/C++ code on all .NET platforms. The [low level API]({{site.url}}/docs/api/001-data-types.html) is exposed through the [static class AmanithSVG](https://github.com/Mazatech/amanithsvg-sdk/blob/master/examples/unity/Assets/SVGAssets/Scripts/SVGAssets/SVGAssets.cs).

 - a high level layer that abstracts some details of the low level API

Here's a list of classes exposed by the high level C# layer:

 - [SVGError](#svgerror)
 - [SVGAlign](#svgalign)
 - [SVGMeetOrSlice](#svgmeetorslice)
 - [SVGRenderingQuality](#svgrenderingquality)
 - [SVGLogLevel](#svgloglevel)
 - [SVGResourceType](#svgresourcetype)
 - [SVGResourceHint](#svgresourcehint)
 - [SVGColor](#svgcolor)
 - [SVGPoint](#svgpoint)
 - [SVGViewport](#svgviewport)
 - [SVGAspectRatio](#svgaspectratio)
 - [SVGDocument](#svgdocument)
 - [SVGSurface](#svgsurface)
 - [SVGPacker](#svgpacker)
 - [SVGAssetsConfig](#svgassetsconfig)
 - [SVGResource](#svgresource)
 - [SVGAssets](#svgassets)

---

## SVGError

The `SVGError` enumeration wraps the native [SVGTErrorCode]({{site.url}}/docs/api/002-errors.html) enum type, and it represents all possible error codes signaled dy AmanithSVG functions.

---

## SVGAlign

The `SVGAlign` enumeration wraps the native `SVGTAspectRatioAlign` enum type, and it defines the alignment method to use in case the aspect ratio of the source (document) viewport doesn't match the aspect ratio of the destination (drawing surface) viewport.

---

## SVGMeetOrSlice

The `SVGMeetOrSlice` enumeration wraps the native `SVGTAspectRatioMeetOrSlice` enum type, and it defines the scaling method to use in case the aspect ratio of the source (document) viewport doesn't match the aspect ratio of the destination (drawing surface) viewport.

---

## SVGRenderingQuality

The `SVGRenderingQuality` enumeration wraps the native `SVGTRenderingQuality` enum type, and it defines the quality (and speed) of SVG drawings.

---

## SVGLogLevel

The `SVGLogLevel` enumeration wraps the native `SVGTLogLevel` enum type, and it defines the possible logging levels (e.g. errors, warnings or just informational messages).

---

## SVGResourceType

The `SVGResourceType` enumeration wraps the native `SVGTResourceType` enum type, and it defines the type of each external resource provided to AmanithSVG.

---

## SVGResourceHint

The `SVGResourceHint` enumeration wraps the native `SVGTResourceHint` enum type, and it defines all the possible hints for tagging external resources provided to AmanithSVG.

---

## SVGColor

`SVGColor` is a simple class that exposes four (float) color components (Red, Green, Blue, Alpha), within the `[0.0f; 1.0f]` range. The class is so simple that it does not require further explanation.

---

## SVGPoint

`SVGPoint` is a simple class representing a 2D point, the class exposes two (float) properties: abscissa (X) and ordinate (Y). The class is so simple that it does not require further explanation.

---

## SVGViewport

A viewport represents a rectangular area, specified by its top/left corner, a width and an height. The positive x-axis points towards the right, the positive y-axis points down.
The class exposes four (float) properties: X, Y, Width, Height. The class is so simple that it does not require further explanation.

---

## SVGAspectRatio

This class represents a couple of values, one taken from the `SVGAlign` enum type and the other taken from the `SVGMeetOrSlice` enum type.
Alignment indicates whether to force uniform scaling and, if so, the alignment method to use in case the aspect ratio of the source viewport doesn't match the aspect ratio of the destination viewport.
For detailed explanation of `SVGAlign` and `SVGMeetOrSlice` enum type, please refer to the [low level API documentation]({{site.url}}/docs/api/007-rendering-svg-document.html#documents-alignment).

| &nbsp; |
| :---: |
| *A brief visual example of possible alignments* |
{:.tbl_images .preserveAspectRatio}

---

## SVGDocument

In order to render SVG contents, AmanithSVG must create an optimized in-memory representation of SVG files: a such representation is called an SVG document.
An SVG document can be created through `SVGAssets.CreateDocument` function, specifying the xml text. The document will be parsed immediately, and the internal drawing tree will be created.
Once the document has been created, it can be drawn several times onto one (or more) drawing surface.
In order to draw a document, we must:

 1. create a drawing surface using `SVGAssets.CreateSurface`
 1. call surface `Draw` method, specifying the document to draw

Example:

```c#
string xml = File.ReadAllText("animals.svg");
SVGDocument doc = SVGAssets.CreateDocument(xml);
```

`SVGDocument` class exposes four main properties: `Handle`, `Width`, `Height`, `Viewport`, `AspectRatio`.

```c#
/* 
   AmanithSVG document handle (read only); it's the handle created by the low level
   svgtDocCreate function.
*/
uint Handle;

/*
    SVG content itself optionally can provide information about the appropriate 
    viewport region for the content via the 'width' and 'height' XML attributes
    on the outermost <svg> element.
    Use this (readonly) property to get the suggested viewport width, in pixels.

    It returns a negative number (i.e. an invalid width) in the following cases:
    - outermost element is not an <svg> element
    - outermost <svg> element doesn't have a 'width' attribute specified
    - outermost <svg> element has a negative 'width' attribute specified
      (e.g. width="-50")
    - outermost <svg> element has a 'width' attribute specified in relative measure
      units (i.e. em, ex, % percentage).
*/
float Width;

/*
    SVG content itself optionally can provide information about the appropriate
    viewport region for the content via the 'width' and 'height' XML attributes on
    the outermost <svg> element.
    Use this (readonly) property to get the suggested viewport height, in pixels.

    It returns -1 (i.e. an invalid height) in the following cases:
    - outermost element is not an <svg> element
    - outermost <svg> element doesn't have a 'height' attribute specified
    - outermost <svg> element has a negative 'height' attribute specified
      (e.g. height="-30")
    - outermost <svg> element has a 'height' attribute specified in relative measure
      units (i.e. em, ex, % percentage).
*/
float Height;

/*
    The document (logical) viewport to map onto the destination (drawing surface)
    viewport. When an SVG document has been created through the SVGAssets.CreateDocument
    function, the initial value of its viewport is equal to the 'viewBox' attribute
    present in the outermost <svg> element.
*/
SVGViewport Viewport;

/*
    Viewport aspect ratio.
    The alignment parameter indicates whether to force uniform scaling and, if so,
    the alignment method to use in case the aspect ratio of the document viewport doesn't
    match the aspect ratio of the surface viewport.
*/
SVGAspectRatio AspectRatio;
```

---

## SVGSurface

`SVGSurface` class represents a drawing surface.

A drawing surface is just a rectangular area made of pixels, where each pixel is represented internally by a 32bit unsigned integer.
A pixel is made of four 8-bit components: red, green, blue, alpha. Coordinate system is the same of SVG specifications: top/left pixel has coordinate (0, 0), with the positive x-axis pointing towards the right and the positive y-axis pointing down.

An `SVGSurface` can be created by using the `SVGAssets.CreateSurface` function, specifying its dimensions in pixels.

`SVGSurface` class exposes four main properties: `Handle`, `Width`, `Height`, `Viewport`.

```c#
/* 
   AmanithSVG surface handle (read only); it's the handle created by the low level
   svgtSurfaceCreate function.
*/
uint Handle;

/* Get current surface width, in pixels. NB: the property is readonly. */
uint Width;

/* Get current surface height, in pixels. NB: the property is readonly. */
uint Height;

/*
    The surface viewport (i.e. a drawing surface rectangular area), where to map the
    source document viewport.
    The combined use of surface and document viewport, induces a transformation matrix,
    that will be used to draw the whole SVG document. The induced matrix grants that
    the document viewport is mapped onto the surface viewport (respecting the specified
    alignment): all SVG content will be drawn accordingly.
*/
SVGViewport Viewport;
```

A drawing surface can be resized by calling the `Resize` method:

```c#
/*
    Resize the surface, specifying new dimensions in pixels.
    After resizing, the surface viewport will be reset to the whole surface.

    It returns SVGError.None if the operation was completed successfully, else
    an error code.
*/
SVGError Resize(uint newWidth, uint newHeight);
```

Once that an `SVGDocument` has been created, it can be rendered over a drawing surface using the following `SVGSurface` method:

```c#
/*
    Draw an SVG document, on this drawing surface.

    First the drawing surface is cleared if a valid (i.e. not null) clear color is
    provided. Then the specified document, if valid, is drawn.

    It returns SVGError.None if the operation was completed successfully, else
    an error code.
*/
SVGError Draw(SVGDocument document,
              SVGColor clearColor,
              SVGRenderingQuality renderingQuality);
```

Here's a complete example:

```c#
/* load an SVG file */
string xml = File.ReadAllText("animals.svg");
SVGDocument doc = SVGAssets.CreateDocument(xml);

/* create a 512x512 drawing surface */
SVGSurface srf = SVGAssets.CreateSurface(512, 512);

/* draw the SVG */
srf.Draw(doc, SVGColor.White, SVGRenderingQuality.Better);
< do something with surface pixels (e.g. upload pixels to a GPU texture) >

/* destroy surface and document */
doc.Dispose();
srf.Dispose();
```

| &nbsp; |
| :---: |
| *Rendering result* |
{:.tbl_images .animalsSvg}

By modifying the drawing surface viewport, it's possible to draw more than one SVG document on it. For example, suppose that we want to draw four different SVG documents within the same drawing surface (of course over 4 different non-overlapping sub-regions), the code would look as follow:

```c#
/* load SVG files */
SVGDocument doc1 = SVGAssets.CreateDocument(File.ReadAllText("car_coolant.svg"));
SVGDocument doc2 = SVGAssets.CreateDocument(File.ReadAllText("car_maintenance.svg"));
SVGDocument doc3 = SVGAssets.CreateDocument(File.ReadAllText("car_brake.svg"));
SVGDocument doc4 = SVGAssets.CreateDocument(File.ReadAllText("car_oil.svg"));

/* create a drawing surface */
SVGSurface srf = SVGAssets.CreateSurface(512, 512);

/* select an upper-left sub-region and draw the first SVG document */
srf.Viewport = new SVGViewport(0, 0, 256, 256);
srf.Draw(doc1, SVGColor.White, SVGRenderingQuality.Better);

/* select an upper-right sub-region and draw the second SVG document */
srf.Viewport = new SVGViewport(256, 0, 256, 256);
srf.Draw(doc2, null, SVGRenderingQuality.Better);

/* select the whole lower-left sub-region and draw the third SVG document */
srf.Viewport = new SVGViewport(0, 256, 256, 256);
srf.Draw(doc3, null, SVGRenderingQuality.Better);

/* select the whole lower-right sub-region and draw the fourth SVG document */
srf.Viewport = new SVGViewport(256, 256, 256, 256);
srf.Draw(doc4, null, SVGRenderingQuality.Better);

< do something with surface pixels (e.g. upload pixels to a GPU texture) >

/* destroy surface and documents */
doc1.Dispose();
doc2.Dispose();
doc3.Dispose();
doc4.Dispose();
srf.Dispose();
```

| &nbsp; |
| :---: |
| *Usage of surface viewport* |
{:.tbl_images .srf_viewport}

---

## SVGPacker

Besides rendering single SVG documents over drawing surfaces, AmanithSVG can pack the rendering of one or more SVG documents within one or more drawing surfaces, automatically. For each given SVG, you can choose to pack the whole document or instead to pack each first-level element separately.

The whole process consists of some steps that must be followed in this order:

 - create an instance of `SVGPacker` class
 - start the packing process using the `SVGPacker.Begin` method
 - add one or more SVG document to the process, using the `SVGPacker.Add` method
 - stop the packing process using the `SVGPacker.End` method
 - for each atlas information (`SVGPackedBin`) returned by the `SVGPacker.End` method:
   - create a drawing surface with the same atlas dimensions, using the `SVGAssets.CreateSurface` function
   - draw the SVG documents/elements packed in the current atlas (`SVGPackedBin`), using the `SVGSurface.Draw` method

Here is a complete example code:

```c#
/* create an SVG document */
SVGDocument doc = SVGAssets.CreateDocument(File.ReadAllText("orc.svg"));

/* initialize the packing process: generate atlases with a maximum 512 pixels
   dimension and no additional scale */
SVGPacker packer = SVGAssets.CreatePacker(1.0f, 512, 1, false);

if (packer.Begin() == SVGError.None) {
    
    /* add the document to the packer, and get back the actual number of
       packed bounding boxes */
    uint[] info = packer.Add(doc, true, 1.0f);

    /* info[0] = number of collected bounding boxes
       info[1] = the actual number of packed bounding boxes */
    if ((info == null) || (info[1] != info[0])) {
        Console.WriteLine("Some SVG elements cannot be packed!");
        Console.WriteLine("Specified maximum texture dimensions do not allow to pack all SVG elements.");
        /* close the packing process without doing anything */
        packer.End(false);
    }
    else {
        /* finalize the packing process and get back generated bins */
        SVGPackedBin[] bins = packer.End(true);

        for (int i = 0; i < bins.Length; ++i) {
            /* extract the bin (i.e. get atlas width, height and the number of packed documents/elements) */
            SVGPackedBin bin = bins[i];
            /* create a drawing surface with the same atlas dimension */
            SVGSurface srf = SVGAssets.CreateSurface(bin.Width, bin.Height);
            /* draw all documents/elements over the drawing surface */
            srf.Draw(bin, SVGColor.Clear, SVGRenderingQuality.Better);
            /* if needed, process the drawing surface (e.g. upload its pixels on a GPU texture) and then destroy it */
            < do something with surface pixels (e.g. upload pixels to a GPU texture) >
            /* destroy the surface */
            srf.Dispose();
        }
    }
}

/* destroy the document */
doc.Dispose();
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

It is important to note that the whole packing process, as well as the `SVGSurface.Draw(SVGPackedBin, SVGColor, SVGRenderingQuality)` method, is not affected by SVG documents viewports nor by drawing surfaces viewports. In other words, the viewport values set through `SVGDocument.Viewport` and `SVGSurface.Viewport` properties do not affect the behavior of packing processes and atlas generation.

For a more detailed explanation on the packing process, please refer to the relative [low level API]({{site.url}}/docs/api/008-packing-atlas-generation.html).

---

## SVGResource

This class represents an external (binary) resource, available for rendering SVG documents. Each resource is defined by a unique string identifier (typically the file name, without the path), a type (font or image), and a set of hints; all such properties must be specified in `SVGResource` constructor. `SVGResource` class cannot be instantiated directly, because it has an abstract `GetBytes` method that must be implemented according to the target platform (or development tool/environment): such method must return an in-memory representation of the underlying binary TTF / OTF / WOFF / JPEG / PNG file.

```c#
/* Constructor. */
abstract SVGResource(string id,
                     SVGResourceType type,
                     SVGResourceHint hints);

/* Get in-memory binary data for the resource. */
abstract byte[] GetBytes();

// Resource identifier.
string Id;
// Resource type.
SVGResourceType Type;
// Resource hints.
SVGResourceHint Hints;

```

Resources are provided to `SVGAssets` through an instance of a class derived from `SVGAssetsConfig`.

---

## SVGAssetsConfig

This class provides configuration parameters, along with resources, to an `SVGAssets` instance. Configuration parameters can be divided in two sub-sets:

- general user-defined parameters: curves quality, user-agent language, log level
- platform-specific parameters: device screen with/height in pixels, screen dpi

The class is `abstract` so it cannot be instantiated directly; it must be extended by implementing the mechanism to access resource files (defined by the `SVGResource` class).
In particular, each class that extends `SVGAssetsConfig` must implement the `ResourcesCount` and `GetResource` methods. See details:

```c#
/*
    Constructor, device screen properties must be supplied
    (with/height in pixels and dpi).
*/
abstract SVGAssetsConfig(uint screenWidth,
                         uint screenHeight,
                         float screenDpi);

/*
    Get the number of (external) resources provided by this configuration.
    It must be implemented by all derived classes.
*/
abstract int ResourcesCount();

/*
    Get a resource given an index.
    If the given index is less than zero or greater or equal to the value
    returned by ResourcesCount, a null resource is returned.

    It must be implemented by all derived classes.
*/
abstract SVGResource GetResource(int index);

/*
    Curves quality, used by AmanithSVG geometric kernel to approximate curves
    with straight line segments (flattening). Valid range is [1; 100], where 100
    represents the best quality.

    A zero or negative value means "keep the default one".
*/
float CurvesQuality;

/*
    User-agent language settings; a list of languages separated by
    semicolon (e.g. "en-US;en-GB;it;es"). It must not be empty.
*/
string Language;

/* Log level, if SVGLogLevel.None is specified, logging is disabled. */
SVGLogLevel LogLevel;

/* Log capacity, in characters; if zero is specified, logging is disabled. */
uint LogCapacity;
```

Because there exists several .NET based windowing/graphics systems (e.g. .NET Framework, Xamarin.Android, Xamarin.iOS, Xamarin.Mac, Unity), the implementation of platform-specific parameters is left to the developer. For Xamarin, it is advisable to take a look at [Xamarin.Essentials DeviceDisplay class](https://docs.microsoft.com/it-it/xamarin/essentials/device-display) and at [Android.Util.DisplayMetrics](https://developer.xamarin.com/api/type/Android.Util.DisplayMetrics/#memberlist). For native methods on how to get screen resolution and dpi, please refer to [this section]({{site.url}}/docs/api/004-initialization-finalization.html).

---

## SVGAssets

`SVGAssets` is a static class through which `SVGDocument`, `SVGSurface` and `SVGPacker` instances can be created. In order to work properly, `SVGAssets` must be initialized in advance through its internal `Init` method:

```c#
/*
    Initialize AmanithSVG native library and load the given configuration.
    The provided settings include screen metrics, user-agent language and
    possible resources (fonts and images).

    It returns SVGError.None if the operation was completed successfully, else
    an error code.
*/
internal SVGError Init(SVGAssetsConfig config);
```

Here are the main methods:

```c#
/*
    Create a drawing surface, specifying its dimensions in pixels.

    Supplied dimensions should be positive numbers greater than zero, else
    a null instance will be returned.
*/
SVGSurface CreateSurface(uint width,
                         uint height);

/* Create and load an SVG document, specifying the whole XML string. */
SVGDocument CreateDocument(string xmlText);

/*
    Create an SVG packer, specifying a scale factor.

    Every collected SVG document/element will be packed into rectangular bins, whose
    dimensions won't exceed the specified 'maxTexturesDimension' in pixels.

    If true, 'pow2Textures' will force bins to have power-of-two dimensions.

    Each rectangle will be separated from the others by the specified 'border' in pixels.

    The specified 'scale' factor will be applied to all collected SVG documents/elements,
    in order to realize resolution-independent atlases.
*/
SVGPacker CreatePacker(float scale,
                       uint maxTexturesDimension,
                       uint border,
                       bool pow2Textures);

/* Get library version. */
string GetVersion();
```

AmanithSVG can provide detailed information about error situations that can occur when parsing and rendering SVG content. For example AmanithSVG can report if a closing tag did not match the opening one (or if some tag was not closed at all) when parsing an SVG document; or if a font needed for rendering has not been set. In case of error, it is possible to receive additional information on the conditions (which caused the error) in the form of text messages that AmanithSVG can append to a given memory buffer.
Log level and the capacity (in characters) of the internal log buffer can be configured through the `SVGAssetsConfig` class, provided to `SVGAssets` initialization mechanism.

```c#
/* Get AmanithSVG log content as a string. */
string LogGet();

/*
    Clear the AmanithSVG log buffer set for the current thread.

    If specified, make sure AmanithSVG no longer uses a log buffer for
    the current thread. In this case, logging can be reactivated (for
    the calling thread) by calling this method with a 'false' parameter.
*/
SVGError LogClear(bool stopLogging);
```

From an application point of view, it could be useful to apppend some custom messages to the AmanithSVG log buffer. One of the main purposes can be to insert debug messages, to better understand the interactions between the application and the AmanithSVG library. In this way the application can keep track of all SVG-related activities, and the whole log buffer can be analyzed later (if needed). Here are the public methods, relative to logging, provided by `SVGAssets`:

```c#
/*
    Append an informational message to the AmanithSVG log buffer
    set for the current thread.
*/
SVGError LogInfo(string message);

/*
    Append a warning message to the AmanithSVG log buffer
    set for the current thread.
*/
SVGError LogWarning(string message);

/*
    Append an error message to the AmanithSVG log buffer
    set for the current thread.
*/
SVGError LogError(string message);
```

---
