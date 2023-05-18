---
layout: default
title: "Android"
date: 2021-01-01 08:00:00 +0100
chapter: 5
categories: [binding]
---

# Android binding

Android binding of AmanithSVG API consists in the extension of the higher layer Java binding already discussed [here]({{site.url}}/docs/binding/002-java.html).

Here's a list of classes exposed by the Android binding:

 - [SVGResourceAndroid](#svgresourceandroid)
 - [SVGAssetsConfigAndroid](#svgassetsconfigandroid)
 - [SVGAssetsAndroid](#svgassetsandroid)

**When working with Android platform, it is mandatory to use `SVGAssetsAndroid` class and not the basic `SVGAssets` one**.

For a complete Android example, please have a look at [the SVG Viewer](https://github.com/Mazatech/amanithsvg-sdk/tree/master/examples/svgViewer).

---

## SVGResourceAndroid

[This class](https://github.com/Mazatech/amanithsvg-sdk/blob/master/examples/svgViewer/platform/android/app/src/main/java/com/mazatech/android/SVGAssetsConfigAndroid.java) extends [SVGResource]({{site.url}}/docs/binding/002-java.html#svgresource) by providing the mandatory `getStream` method to get an in-memory representation of the underlying binary TTF / OTF / WOFF / JPEG / PNG file. The constructor takes an [InputStream](https://developer.android.com/reference/java/io/InputStream) and sets the given unique string identifier, the type (font or image) and hints.

---

## SVGAssetsConfigAndroid

[This class](https://github.com/Mazatech/amanithsvg-sdk/blob/master/examples/svgViewer/platform/android/app/src/main/java/com/mazatech/android/SVGAssetsConfigAndroid.java) extends [SVGAssetsConfig]({{site.url}}/docs/binding/002-java.html#svgassetsconfig) by implementing an internal list of `SVGResourceAndroid` resources. Most importantly this class implements the mandatory `resourcesCount` and `getResource` methods from `SVGAssetsConfig` class, in order to get the resources.

```java
/*
    Constructor, device screen properties must be supplied
    (with/height in pixels and dpi).
*/
SVGAssetsConfigAndroid(int screenWidth,
                       int screenHeight,
                       float screenDpi);

/*
    Add a font resource to the configuration, by providing an input stream.
    Return true if the resource has been added to the internal list, else
    false (i.e. the resource was already present within the internal list).
*/
boolean addFont(InputStream resourceStream,
                // the unique integer id as generate by aapt tool
                int aaptId,
                // a unique not-empty string identifier, provided by the
                // application (typically the file name, without the path)
                String strId,
                EnumSet<SVGTResourceHint> hints);

/*
    Add an image resource to the configuration, by providing an input stream.
    Return true if the resource has been added to the internal list, else
    false (i.e. the resource was already present within the internal list).
*/
boolean addImage(InputStream resourceStream,
                 // the unique integer id as generate by aapt tool
                 int aaptId,
                 // a unique not-empty string identifier, provided by the
                 // application (typically the file name, without the path)
                 String strId);

```

All font resources must be specified before the instantiation of `SVGAssetsAndroid` class. Here's an example:

```java
DisplayMetrics metrics = getResources().getDisplayMetrics();
SVGAssetsConfigAndroid cfg = new SVGAssetsConfigAndroid(metrics.widthPixels,
                                                        metrics.heightPixels,
                                                        metrics.xdpi);

/* provide fonts to AmanithSVG, in order to render <text> elements */
cfg.addFont(getResources().openRawResource(R.raw.verdana),
            // the unique integer id as generate by aapt tool
            R.raw.verdana,
            // a unique not-empty string identifier
            "Verdana",
            EnumSet.of(SVGTResourceHint.DefaultSerif,
                       SVGTResourceHint.DefaultUISerif));
cfg.addFont(getResources().openRawResource(R.raw.arialb),
            // the unique integer id as generate by aapt tool
            R.raw.arialb,
            // a unique not-empty string identifier
            "ArialBold");

/* initialize AmanithSVG for Android */
SVGAssetsAndroid svg = new SVGAssetsAndroid(config);

```

---

## SVGAssetsAndroid

[This class](https://github.com/Mazatech/amanithsvg-sdk/blob/master/examples/svgViewer/platform/android/app/src/main/java/com/mazatech/android/SVGAssetsAndroid.java) extends [SVGAssets]({{site.url}}/docs/binding/002-java.html#svgassets) class by implementing a set of specific functions to create OpenGL ES textures out of [SVGSurface]({{site.url}}/docs/binding/002-java.html#svgsurface). The class is also in charge of loading the AmanithSVG native library (JNI) for all Android supported platforms.

```java
/* Constructor, the given configuration must provide resources too. */
SVGAssetsAndroid(SVGAssetsConfigAndroid config);

/* Create and load an SVG document, specifying a full file path (path and file name). */
SVGDocument createDocumentFromFile(String filePath);

/*
    Create an OpenGL ES texture out of the given drawing surface.

    Internally the method queries OpenGL ES runtime, in order to verify the support
    of helpful extensions that can accelerate the upload of AmanithSVG surface
    pixels to the OpenGL ES texture. Tested GPU features are:

    - BGRA textures: GL_APPLE_texture_format_BGRA8888, GL_EXT_bgra,
      GL_IMG_texture_format_BGRA8888, GL_EXT_texture_format_BGRA8888

    - NPOT (non-power-of-two) textures: GL_OES_texture_npot,
      GL_APPLE_texture_2D_limited_npot, GL_ARB_texture_non_power_of_two

    - texture swizzle: GL_EXT_texture_swizzle, GL_ARB_texture_swizzle

    The function returns the OpenGL ES texture handle.
*/
int createTexture(GL11 gl,
                  SVGSurface surface);
```

---
