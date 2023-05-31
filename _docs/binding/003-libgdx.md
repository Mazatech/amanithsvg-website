---
layout: default
title: "libGDX"
date: 2018-01-01 08:00:00 +0100
chapter: 3
categories: [binding]
---

# libGDX binding

[libGDX](http://libgdx.com) is a light, free, open source cross platform game development framework. The goal of the project is to assist the developer in creating games/applications and deploy to desktop and mobile platforms without getting in the way and letting you design however you like.

The whole libGDX graphics stack is based upon bitmaps (i.e. textures) and their sub-regions (i.e. sprites), so AmanithSVG is a complementary extension that could be well integrated within the current libGDX API.

In particular, the libGDX binding of AmanithSVG API consists of the extension of some libGDX classes, by using the higher layer Java binding already discussed [here]({{site.url}}/docs/binding/002-java.html).

Here's a list of classes exposed by the libGDX binding:

 - [SVGResourceGDX](#svgresourcegdx)
 - [SVGAssetsConfigGDX](#svgassetsconfiggdx)
 - [SVGAssetsGDX](#svgassetsgdx)
 - [SVGTexture](#svgtexture)
 - [SVGTextureAtlasGenerator](#svgtextureatlasgenerator)
 - [SVGTextureAtlas](#svgtextureatlas)
 - [SVGTextureAtlasPage](#svgtextureatlaspage)
 - [SVGTextureAtlasRegion](#svgtextureatlasregion)

**When working with libGDX engine, it is mandatory to use `SVGAssetsGDX` class and not the basic `SVGAssets` one**.

For a complete libGDX example, please have a look at [the Cards Game](http://github.com/Mazatech/amanithsvg-sdk/tree/master/examples/libgdx/gameCards).

---

## SVGResourceGDX

[This class](https://github.com/Mazatech/amanithsvg-sdk/blob/master/examples/libgdx/gameCards/core/src/com/mazatech/gdx/SVGAssetsConfigGDX.java) extends [SVGResource]({{site.url}}/docs/binding/002-java.html#svgresource) by providing the mandatory `getStream` method to get an in-memory representation of the underlying binary TTF / OTF / WOFF / JPEG / PNG file. The constructor takes a [FileHandle](https://javadoc.io/static/com.badlogicgames.gdx/gdx/1.11.0/com/badlogic/gdx/files/FileHandle.html) and sets the given unique string identifier, the type (font or image) and hints. The access to the underlying binary data is implemented by using the `FileHandle.read` method.

---

## SVGAssetsConfigGDX

[This class](https://github.com/Mazatech/amanithsvg-sdk/blob/master/examples/libgdx/gameCards/core/src/com/mazatech/gdx/SVGAssetsConfigGDX.java) extends [SVGAssetsConfig]({{site.url}}/docs/binding/002-java.html#svgassetsconfig) by implementing an internal list of `SVGResourceGDX` resources. Most importantly this class implements the mandatory `resourcesCount` and `getResource` methods from `SVGAssetsConfig` class, in order to get the resources.


```java
/*
    Constructor, device screen properties must be supplied
    (with/height in pixels and dpi).
*/
SVGAssetsConfigGDX(int screenWidth,
                   int screenHeight,
                   float screenDpi);

/*
    Add a font resource to the configuration, by providing its internal path.
    Return true if the resource has been added to the internal list, else
    false (i.e. the resource was already present within the internal list).
*/
boolean addFont(String internalPath,
                // a unique not-empty string identifier, provided by the
                // application (typically the file name, without the path)
                String strId,
                EnumSet<SVGTResourceHint> hints);

/*
    Add a font resource to the configuration, by providing its file handle.
    Return true if the resource has been added to the internal list, else
    false (i.e. the resource was already present within the internal list).
*/
boolean addFont(FileHandle fileHandle,
                // a unique not-empty string identifier, provided by the
                // application (typically the file name, without the path)
                String strId,
                EnumSet<SVGTResourceHint> hints);

/*
    Add an image resource to the configuration, by providing its internal path.
    Return true if the resource has been added to the internal list, else
    false (i.e. the resource was already present within the internal list).
*/
boolean addImage(String internalPath,
                // a unique not-empty string identifier, provided by the
                // application (typically the file name, without the path)
                String strId);

/*
    Add an image resource to the configuration, by providing its file handle.
    Return true if the resource has been added to the internal list, else
    false (i.e. the resource was already present within the internal list).
*/
boolean addImage(FileHandle fileHandle,
                 // a unique not-empty string identifier, provided by the
                 // application (typically the file name, without the path)
                 String strId);

```

All font resources must be specified before the instantiation of `SVGAssetsGDX` class. Here's an example:

```java
/* create configuration for AmanithSVG */
SVGAssetsConfigGDX cfg = new SVGAssetsConfigGDX(Gdx.graphics.getBackBufferWidth(),
                                                Gdx.graphics.getBackBufferHeight(),
                                                Gdx.graphics.getPpiX());

/* provide fonts to AmanithSVG, in order to render <text> elements */
cfg.addFont("verdana.ttf", "Verdana", EnumSet.of(SVGTResourceHint.DefaultSerif,
                                                 SVGTResourceHint.DefaultUISerif));
cfg.addFont("arialb.ttf", "ArialBold");

/* initialize AmanithSVG for libGDX */
SVGAssetsGDX svg = new SVGAssetsGDX(cfg);

< do something with svg >

/* release AmanithSVG */
svg.dispose();
```

---

## SVGAssetsGDX

[This class](https://github.com/Mazatech/amanithsvg-sdk/blob/master/examples/libgdx/gameCards/core/src/com/mazatech/gdx/SVGAssetsGDX.java) extends [SVGAssets]({{site.url}}/docs/binding/002-java.html#svgassets) class by implementing a set of specific functions to create `SVGTexture` and `SVGTextureAtlas` instances.
The class is also in charge of loading the AmanithSVG native library for all libGDX supported platforms.

```java
/* Constructor, the given configuraration must provide resources too. */
SVGAssetsGDX(SVGAssetsConfigGDX config);

/*
    Generate a texture from the given SVG file handle.
    Size of texture is specified by the given 'width' and 'height'.
    Before the SVG rendering, pixels are initialized with the given 'clearColor'.

    If the 'dilateEdgesFix' flag is set to true, the rendering process will also
    perform a 1-pixel dilate post-filter; this dilate filter is useful when the
    texture has some transparent parts (i.e. pixels with alpha component = 0): such
    flag will induce TextureFilter.Linear minification/magnification filtering.

    If the 'dilateEdgesFix' flag is set to false, no additional dilate post-filter
    is applied, and the texture minification/magnification filtering is set to
    TextureFilter.Nearest.
*/
SVGTexture createTexture(FileHandle svgFile,
                         int width,
                         int height,
                         SVGColor clearColor,
                         boolean dilateEdgesFix);

/*
    Create a texture out of an SVG document (already instantiated externally).
    Size of texture is specified by the given 'width' and 'height'.
    Before the SVG rendering, pixels are initialized with the given 'clearColor'.

    If the 'dilateEdgesFix' flag is set to true, the rendering process will also
    perform a 1-pixel dilate post-filter; this dilate filter is useful when the
    texture has some transparent parts (i.e. pixels with alpha component = 0): such
    flag will induce TextureFilter.Linear minification/magnification filtering.

    If the 'dilateEdgesFix' flag is set to false, no additional dilate post-filter
    is applied, and the texture minification/magnification filtering is set to
    TextureFilter.Nearest.
*/
SVGTexture createTexture(SVGDocument doc,
                         int width,
                         int height,
                         SVGColor clearColor,
                         boolean dilateEdgesFix);

/*
    Instantiate a texture atlas generator.

    The specified `scale` factor will be applied to all collected SVG
    documents/elements, in order to realize resolution-independent atlas.
    Every collected SVG document/element will be packed into rectangular
    textures, whose dimensions won't exceed the specified 'maxTexturesDimension',
    in pixels.

    If true, 'pow2Textures' will force textures to have power-of-two dimensions.

    Each packed element will be separated from the others by the specified
    'border', in pixels.

    If the 'dilateEdgesFix' flag is set to true, the rendering process will also
    perform a 1-pixel dilate post-filter; this dilate filter is useful when the
    texture has some transparent parts (i.e. pixels with alpha component = 0): such
    flag will induce TextureFilter.Linear minification/magnification filtering.

    If the 'dilateEdgesFix' flag is set to false, no additional dilate post-filter
    is applied, and the texture minification/magnification filtering is set to
    TextureFilter.Nearest.

    Before the SVG rendering, pixels are initialized with the given 'clearColor'.
*/
SVGTextureAtlasGenerator createAtlasGenerator(float scale,
                                              int maxTexturesDimension,
                                              int border,
                                              boolean pow2Textures,
                                              boolean dilateEdgesFix,
                                              SVGColor clearColor);
```

In addition this class implements a method to create a `SVGDocument` instance from files:

```java
/* Create and load an SVG document, specifying a file handle. */
SVGDocument createDocument(FileHandle file);

/*
    Create and load an SVG document, specifying an internal filename.
    It is a simple shortcut to the previous method:

    createDocument(Gdx.files.internal(internalFileName))
*/
SVGDocument createDocumentFromFile(String internalFileName);
```

Here is an example of `SVGAssetsGDX` initialization:

```java
/* create configuration for AmanithSVG */
SVGAssetsConfigGDX cfg = new SVGAssetsConfigGDX(Gdx.graphics.getBackBufferWidth(),
                                                Gdx.graphics.getBackBufferHeight(),
                                                Gdx.graphics.getPpiX());
/* initialize AmanithSVG for libGDX */
SVGAssetsGDX svg = new SVGAssetsGDX(cfg);

< do something with svg >

/* release AmanithSVG */
svg.dispose();
```

---

## SVGTexture

[SVGTexture](https://github.com/Mazatech/amanithsvg-sdk/blob/master/examples/libgdx/gameCards/core/src/com/mazatech/gdx/SVGTexture.java) class extends the libGDX [Texture class](https://javadoc.io/static/com.badlogicgames.gdx/gdx/1.11.0/com/badlogic/gdx/graphics/Texture.html). Therefore `SVGTexture` is a Texture whose content (i.e. pixels) is created by the rendering of an SVG. Compared to the base class, `SVGTexture` does not expose additional methods, but just a bunch of new constructors, specifically designed to provide SVG content.

The class can be instantiated by `SVGAssetsGDX`'s `createTexture` methods, here is an example:

```java
/* create configuration for AmanithSVG */
SVGAssetsConfigGDX cfg = new SVGAssetsConfigGDX(Gdx.graphics.getBackBufferWidth(),
                                                Gdx.graphics.getBackBufferHeight(),
                                                Gdx.graphics.getPpiX());
/* initialize AmanithSVG for libGDX */
SVGAssetsGDX svg = new SVGAssetsGDX(cfg);

/* create a 512 x 512 texture from animals.svg file */
SVGTexture tex = svg.createTexture("animals.svg", 512, 512);

< do something with the texture, for example batch.draw >

/* release AmanithSVG and resources */
tex.dispose();
svg.dispose();
```

As an additional option, the class can also be instantiated directly by providing an [SVGSurface]({{site.url}}/docs/binding/002-java.html#svgsurface).

```java
/*
    Create a texture out of a drawing surface

    If the 'dilateEdgesFix' flag is set to true, the rendering process will also
    perform a 1-pixel dilate post-filter; this dilate filter is useful when the
    texture has some transparent parts (i.e. pixels with alpha component = 0): such
    flag will induce TextureFilter.Linear minification/magnification filtering.

    If the 'dilateEdgesFix' flag is set to false, no additional dilate post-filter
    is applied, and the texture minification/magnification filtering is set to
    TextureFilter.Nearest.
*/
SVGTexture(SVGSurface surface,
           boolean dilateEdgesFix);
```

This kind of approach is useful when you want to manipulate the surface using the Java binding, and then create a texture on the fly out of the `SVGSurface`.
For example, suppose that we want to draw four different SVG documents within the same drawing surface (of course over 4 different non-overlapping sub-regions), the code would look as follow:

```java
/* create configuration for AmanithSVG */
SVGAssetsConfigGDX cfg = new SVGAssetsConfigGDX(Gdx.graphics.getBackBufferWidth(),
                                                Gdx.graphics.getBackBufferHeight(),
                                                Gdx.graphics.getPpiX());
/* initialize AmanithSVG for libGDX */
SVGAssetsGDX svg = new SVGAssetsGDX(cfg);

/* load SVG files */
SVGDocument doc1 = svg.createDocumentFromFile("car_coolant.svg");
SVGDocument doc2 = svg.createDocumentFromFile("car_maintenance.svg");
SVGDocument doc3 = svg.createDocumentFromFile("car_brake.svg");
SVGDocument doc4 = svg.createDocumentFromFile("car_oil.svg");

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

/* create a texture out of the drawing surface */
SVGTexture tex = new SVGTexture(srf, false);

< do something with the texture, for example batch.draw >

/* destroy documents and surface */
doc1.dispose();
doc2.dispose();
doc3.dispose();
doc4.dispose();
srf.dispose();
tex.dispose();
/* release AmanithSVG */
svg.dispose();
```

| &nbsp; |
| :---: |
| *Usage of surface viewport* |
{:.tbl_images .srf_viewport}

---

## SVGTextureAtlasGenerator

Besides rendering single SVG documents over drawing surfaces, AmanithSVG can pack the rendering of one or more SVG documents within one or more drawing surfaces, automatically. For each given SVG, you can choose to pack the whole document or instead to pack each first-level element separately. The generation of texture atlas has been exposed through the [SVGTextureAtlasGenerator](https://github.com/Mazatech/amanithsvg-sdk/blob/master/examples/libgdx/gameCards/core/src/com/mazatech/gdx/SVGTextureAtlasGenerator.java) class, that can be instantiated through `SVGAssetsGDX`'s `createAtlasGenerator` methods:

```java
SVGTextureAtlasGenerator createAtlasGenerator(float scale,
                                              int maxTexturesDimension,
                                              int border,
                                              boolean pow2Textures,
                                              boolean dilateEdgesFix,
                                              SVGColor clearColor);
```

The specified `scale` factor will be applied to all collected SVG documents/elements, in order to realize resolution-independent atlas. Every collected SVG document/element will be packed into rectangular bins, whose dimensions won't exceed the specified `maxTexturesDimension`, in pixels. If true, `pow2Textures` will force bins to have power-of-two dimensions. Each packed element will be separated from the others by the specified `border`, in pixels. If the `dilateEdgesFix` flag is set to true, the rendering process will also perform a 1-pixel dilate post-filter; this dilate filter is useful when the texture has some transparent parts (i.e. pixels with alpha component = 0): such flag will induce `TextureFilter.Linear` minification/magnification filtering.
If the `dilateEdgesFix` flag is set to false, no additional dilate post-filter is applied, and the texture minification/magnification filtering is set to `TextureFilter.Nearest`.

SVG files can be added to the packing process using the `add` method:

```java
/* Add an SVG document to the current packing process. */
boolean add(FileHandle file,
            boolean explodeGroups,
            float scale);
```

If true, `explodeGroups` tells the packer to not pack the whole SVG document, but instead to pack each first-level element separately; the additional `scale` is used to adjust the document content to the other documents involved in the current packing process.
When all documents have been added, the generation of texture atlas can be performed by calling the `generateAtlas` method:

```java
/*
    Generate the texture atlas; NB: this method MUST be called from
    the OpenGL thread, because it creates textures.
*/
SVGTextureAtlas generateAtlas();
```

Here's an example:

```java
/* create configuration for AmanithSVG */
SVGAssetsConfigGDX cfg = new SVGAssetsConfigGDX(Gdx.graphics.getBackBufferWidth(),
                                                Gdx.graphics.getBackBufferHeight(),
                                                Gdx.graphics.getPpiX());
/* initialize AmanithSVG for libGDX */
SVGAssetsGDX svg = new SVGAssetsGDX(cfg);

/* create a texture atlas generator, textures will have a max dimension of 512 pixels */
SVGTextureAtlasGenerator atlasGen = svg.createAtlasGenerator(1.0f, 512, 1, false,
                                                             true, SVGColor.Clear);
/* explode all first-level groups (i.e. all orc parts, separately) */
atlasGen.add("orc.svg", true, 1.0f);

/* generate the texture atlas */
SVGTextureAtlas atlas = atlasGen.generateAtlas();
/* get access to the generated textures */
SVGTextureAtlasPage[] textures = atlas.getPages();

< do something with generated textures and their sub-regions >

/* release resources */
atlas.dispose();
atlasGen.dispose();
/* release AmanithSVG */
svg.dispose();
```

If we run the example code using this [orc.svg file](https://github.com/Mazatech/amanithsvg-sdk/blob/master/examples/unity/Assets/SVGAssets/SVGFiles/orc.svg.txt), it will produce a single `452 x 108` texture, with the following regions:

|          | elemName | originalX | originalY | x   | y   | width | height | zOrder |
| -------- | -------- | --------- | --------- | --- | --- | ----- | ------ | ------ |
| regions[0] | head | 5 | 9 | 0 | 0 | 133 | 105 | 9 |
| regions[1] | body | 26 | 80 | 133 | 0 | 93 | 97 | 7 |
| regions[2] | sx_leg_down | 75 | 191 | 226 | 0 | 48 | 61 | 2 |
| regions[3] | dx_leg_down | 41 | 191 | 274 | 0 | 38 | 63 | 4 |
| regions[4] | sx_arm_up | 95 | 102 | 312 | 0 | 38 | 54 | 5 |
| regions[5] | dx_arm_up | 19 | 98 | 350 | 0 | 38 | 54 | 6 |
| regions[6] | dx_arm_down | 19 | 141 | 388 | 0 | 32 | 68 | 8 |
| regions[7] | sx_arm_down | 99 | 139 | 420 | 0 | 32 | 68 | 0 |
| regions[8] | dx_leg_up | 43 | 147 | 312 | 54 | 38 | 54 | 3 |
| regions[9] | sx_leg_up | 70 | 146 | 350 | 54 | 38 | 54 | 1 |
{:.rwd-table2 .rwd-table-orcAtlas}

| &nbsp; |
| :---: |
| *orc.svg* |
{:.tbl_images .orcSvg}

| &nbsp; |
| :---: |
| *orc atlas* |
{:.tbl_images .orcSvgAtlas}

---

## SVGTextureAtlas

[SVGTextureAtlas](https://github.com/Mazatech/amanithsvg-sdk/blob/master/examples/libgdx/gameCards/core/src/com/mazatech/gdx/SVGTextureAtlas.java) class represents the result of a packing process performed by an `SVGTextureAtlasGenerator` instance.
The only useful method is the `getPages`, which returns an array of textures:

```java
/* get the generated textures */
SVGTextureAtlasPage[] getPages();
```

---

## SVGTextureAtlasPage

[SVGTextureAtlasPage](https://github.com/Mazatech/amanithsvg-sdk/blob/master/examples/libgdx/gameCards/core/src/com/mazatech/gdx/SVGTextureAtlasPage.java) extends the libGDX [Texture class](https://javadoc.io/static/com.badlogicgames.gdx/gdx/1.11.0/com/badlogic/gdx/graphics/Texture.html).
Because the texture can contain several packed elements from SVG files, it could be useful to access these regions:

```java
/* returns all regions within the texture */
SVGTextureAtlasRegion[] getRegions();
```

---

## SVGTextureAtlasRegion

[SVGTextureAtlasRegion](https://github.com/Mazatech/amanithsvg-sdk/blob/master/examples/libgdx/gameCards/core/src/com/mazatech/gdx/SVGTextureAtlasRegion.java) extends the libGDX [TextureRegion class](https://javadoc.io/static/com.badlogicgames.gdx/gdx/1.11.0/com/badlogic/gdx/graphics/g2d/TextureRegion.html).
In addition to the base class methods, it exposes some useful information about the packed element:

```java
/* 'id' attribute taken from the SVG element, empty if not present */
String getElemName();
/* the horizontal position within the original SVG */
int getOriginalX();
/* the vertical position within the original SVG */
int getOriginalY();
/* the horizontal position within the texture */
int getX();
/* the vertical position within the texture */
int getY();
/* the document (handle) to which this element belongs */
SVGTHandle getDocHandle();
/* Z-order (i.e. the rendering order of the element, as induced by the SVG tree) */
int getZOrder();
/* the used destination viewport width (induced by packing scale factor) */
float getDstViewportWidth();
/* the used destination viewport height (induced by packing scale factor) */
float getDstViewportHeight();
```

---

# libGDX binding FAQ

## "\<xyz\> elements are not supported in AmanithSVG Lite version" messages

Because some features are not present in AmanithSVG Lite, you could see some warning messages about SVG unsupported elements in the Android Studio run console window. In particular the following SVG elements are not supported in AmanithSVG Lite: [\<linearGradient\>](https://www.w3.org/TR/SVG11/pservers.html#LinearGradients), [\<radialGradient\>](https://www.w3.org/TR/SVG11/pservers.html#RadialGradients), [\<pattern\>](https://www.w3.org/TR/SVG11/pservers.html#Patterns), [\<image\>](https://www.w3.org/TR/SVG11/struct.html#ImageElement), [\<mask\>](https://www.w3.org/TR/SVG11/masking.html#Masking), [\<filter\>](https://www.w3.org/TR/SVG11/filters.html). Such elements are supported in AmanithSVG Full only, that is available for licensing on Mazatech's website. In order to use the Full version, it is not needed to change Java code, just substitute the native library.

For more information, please visit [the dedicated page]({{site.url}}/docs/desc/004-lite-vs-full.html).

---
