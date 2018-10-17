---
layout: default
title: "LibGDX"
date: 2018-01-01 08:00:00 +0100
chapter: 3
categories: [binding]
---

# LibGDX binding

[libGDX](http://libgdx.badlogicgames.com) is a light, free, open source cross platform game development framework. The goal of the project is to assist the developer in creating games/applications and deploy to desktop and mobile platforms without getting in the way and letting you design however you like.

The whole libGDX graphics stack is based upon bitmaps (i.e. textures) and their sub-regions (i.e. sprites), so AmanithSVG is a complementary extension that could be well integrated within the current libGDX API.

In particular, the libGDX binding of AmanithSVG API consists of the extension of some libGDX classes, by using the higher layer Java binding already discussed [here]({{site.url}}/docs/binding/002-java.html).

Here's a list of classes exposed by the libGDX binding:

 - [SVGTexture](#svgtexture)
 - [SVGTextureAtlasGenerator](svgtextureatlasgenerator)
 - [SVGTextureAtlas](svgtextureatlas)
 - [SVGTextureAtlasPage](svgtextureatlaspage)
 - [SVGTextureAtlasRegion](svgtextureatlasregion)


---

## SVGTexture

`SVGTexture` class extends the libGDX [Texture class](http://libgdx.badlogicgames.com/ci/nightlies/docs/api/com/badlogic/gdx/graphics/Texture.html). Therefore `SVGTexture` is a Texture whose content (i.e. pixels) is created by the rendering of an SVG. Compared to the base class, `SVGTexture` does not expose additional mathods, but just a bunch of new constructors, specifically designed to provide SVG content:

```java
/*
    Generate a texture from the given "internal" SVG filename.

    With the term "internal", it's intended those read-only files located on
    the internal storage.
    For more details about libGDX file handling, please refer to the official
    documentation (http://github.com/libgdx/libgdx/wiki/File-handling)

    Size of the texture is derived from the information available within the SVG file:

    - if the outermost <svg> element has 'width' and 'height' attributes, such
      values are used to size the texture
    - if the outermost <svg> element does not have 'width' and 'height' attributes,
      the size of texture is determined by the width and height values of the 
      'viewBox' attribute
*/
SVGTexture(String internalPath);

/*
    Generate a texture from the given "internal" SVG filename.
    Size of texture is specified by the given 'width' and 'height'.
    Before the SVG rendering, pixels are initialized with a transparent black.
*/
SVGTexture(String internalPath, int width, int height);

/*
    Generate a texture from the given "internal" SVG filename.
    Size of texture is specified by the given 'width' and 'height'.
    Before the SVG rendering, pixels are initialized with the given 'clearColor'.
*/
SVGTexture(String internalPath, int width, int height, SVGColor clearColor);

/*
    Generate a texture from the given "internal" SVG filename.
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
SVGTexture(String internalPath, 
           int width, int height,
           SVGColor clearColor,
           boolean dilateEdgesFix);
```

There are four additional constructors, with the same meaning of those already described but with the difference that as a first parameter they take a [FileHandle](https://libgdx.badlogicgames.com/ci/nightlies/docs/api/com/badlogic/gdx/files/FileHandle.html):

```java
SVGTexture(FileHandle file);

SVGTexture(FileHandle file, int width, int height);

SVGTexture(FileHandle file, int width, int height,
           SVGColor clearColor);

SVGTexture(FileHandle file, int width, int height,
           SVGColor clearColor, boolean dilateEdgesFix);
```

Some usage examples:

```java
SVGTexture tex = new SVGTexture(Gdx.files.internal("animals.svg"));

SVGTexture tex = new SVGTexture("background.svg",
                                Gdx.graphics.getBackBufferWidth(),
                                Gdx.graphics.getBackBufferHeight());
```

All listed constructors create an [SVGDocument]({{site.url}}/docs/binding/002-java.html#svgdocument) from the given SVG file, and keep it in memory until the texture is disposed.
Such internal `SVGDocument` instance will be used to re-create the texture when the EGL/GL context is lost.
It's also possible to create a texture from a given (external) `SVGDocument`:

```java
SVGTexture(SVGDocument doc, int width, int height,
           SVGColor clearColor, boolean dilateEdgesFix);
```

Here's a possible usage example:

```java
/* SVG background */
SVGDocument[] backgrounds = { null, null, null, null };

/* load/create backgrounds documents (init time, the create() function) */
backgrounds[0] = SVGAssets.createDocument(Gdx.files.internal("gameBkg1.svg"));
backgrounds[1] = SVGAssets.createDocument(Gdx.files.internal("gameBkg2.svg"));
backgrounds[2] = SVGAssets.createDocument(Gdx.files.internal("gameBkg3.svg"));
backgrounds[3] = SVGAssets.createDocument(Gdx.files.internal("gameBkg4.svg"));

/* when a new game level starts, select a background according to 
   the current game state */
int selected = selectBackground(gameState);
/* generate the background texture */
SVGTexture tex = new SVGTexture(backgrounds[selected],
                                Gdx.graphics.getBackBufferWidth(),
                                Gdx.graphics.getBackBufferHeight(),
                                SVGColor.Clear, false);

/* display background (runtime, the render() function) */
batch.draw(tex, ...);

/* at dispose time (the dispose() function), lets free resources */
tex.dispose();
for (int i = 0; i < 4; ++i) {
    backgrounds[i].dispose();
}
```

Finally, as a last option, it is possible to create a texture from a given (external) [SVGSurface]({{site.url}}/docs/binding/002-java.html#svgsurface):

```java
SVGTexture(SVGSurface surface);

SVGTexture(SVGSurface surface, boolean dilateEdgesFix);
```

This kind of approach is useful when you want to manipulate the surface using the Java binding, and then create a texture on the fly out of the `SVGSurface`.
For example, suppose that we want to draw four different SVG documents within the same drawing surface (of course over 4 different non-overlapping sub-regions), the code would look as follow:

```java
/* load SVG files */
SVGDocument svg1 = SVGAssets.createDocument(Gdx.files.internal("car_coolant.svg"));
SVGDocument svg2 = SVGAssets.createDocument(Gdx.files.internal("car_maintenance.svg"));
SVGDocument svg3 = SVGAssets.createDocument(Gdx.files.internal("car_brake.svg"));
SVGDocument svg4 = SVGAssets.createDocument(Gdx.files.internal("car_oil.svg"));

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

/* create a texture out of the drawing surface */
SVGTexture tex = new SVGTexture(srf, false);
< do something with the texture, for example batch.draw >

/* destroy surface and documents */
svg1.dispose();
svg2.dispose();
svg3.dispose();
svg4.dispose();
srf.dispose();
tex.dispose();
```

| ![Surface Viewport ]({{site.url}}/assets/images/srf_viewport.png) | 
| :---: |
| *Usage of surface viewport* |


---

## SVGTextureAtlasGenerator

Besides rendering single SVG documents over drawing surfaces, AmanithSVG can pack the rendering of one or more SVG documents within one or more drawing surfaces, automatically. For each given SVG, you can choose to pack the whole document or instead to pack each first-level element separately. The generation of texture atlas has been exposed through the `SVGTextureAtlasGenerator` class:

```java
/* constructor */
SVGTextureAtlasGenerator(float scale,
                         int maxTexturesDimension,
                         int border,
                         boolean pow2Textures,
                         boolean dilateEdgesFix,
                         SVGColor clearColor);
```

The specified `scale` factor will be applied to all collected SVG documents/elements, in order to realize resolution-independent atlas. Every collected SVG document/element will be packed into rectangular bins, whose dimensions won't exceed the specified `maxTexturesDimension`, in pixels. If true, `pow2Textures` will force bins to have power-of-two dimensions. Each packed element will be separated from the others by the specified `border`, in pixels.

SVG files can be added to the packing process using the `add` method:

```java
/* add an SVG document to the current packing process */
boolean add(FileHandle file, boolean explodeGroups, float scale);
```

If true, `explodeGroups` tells the packer to not pack the whole SVG document, but instead to pack each first-level element separately; the additional `scale` is used to adjust the document content to the other documents involved in the current packing process.
When all documents have been added, the generation of texture atlas can be performed by calling the `generateAtlas` method:

```java
/* generate the texture atlas; NB: this method MUST be called from
   the OpenGL thread, because it creates textures */
SVGTextureAtlas generateAtlas();
```

Here's an example:

```java
SVGTextureAtlasGenerator atlasGen = new SVGTextureAtlasGenerator(1.0f, 512, 1, false,
                                                                 true, SVGColor.Clear);
/* explode all first-level groups (i.e. all orc parts, separately) */
atlasGen.add(Gdx.files.internal("orc.svg"), true, 1.0f);
/* generate the texture atlas */
SVGTextureAtlas atlas = atlasGen.generateAtlas();
/* get access to the generated textures */
SVGTextureAtlasPage[] textures = atlas.getPages();

< do something with generated textures and their sub-regions >

/* release resources */
atlas.dispose();
atlasGen.dispose();
```

If we run the example code using this [orc.svg file](http://github.com/Mazatech/amanithsvg-bindings/blob/master/Unity/Assets/SVGAssets/SVGFiles/orc.svg.txt), it will produce a single `452 x 108` texture, with the following regions:

|           | elemName | originalX | originalY | x    | y    | width | height | zOrder |
| :-------- | :------- | --------: | --------: | ---: | ---: | ----: | -----: | -----: |
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
{:.rwd-table2}

| ![Orc svg]({{site.url}}/assets/images/orc.png) | 
| :---: |
| *orc.svg* |

| ![Orc atlas]({{site.url}}/assets/images/orc_atlas.png) | 
| :---: |
| *orc atlas* |


---

## SVGTextureAtlas

`SVGTextureAtlas` class represents the result of a packing process performed by an `SVGTextureAtlasGenerator` instance.
The only useful method is the `getPages`, which returns an array of texture:

```java
/* get the generated textures */
SVGTextureAtlasPage[] getPages();
```


---

## SVGTextureAtlasPage

`SVGTextureAtlasPage` extends the libGDX [Texture class](http://libgdx.badlogicgames.com/ci/nightlies/docs/api/com/badlogic/gdx/graphics/Texture.html).
Because the texture can contain several packed elements from SVG files, it could be useful to access these regions:

```java
/* returns all regions within the texture */
SVGTextureAtlasRegion[] getRegions();
```


---

## SVGTextureAtlasRegion

`SVGTextureAtlasRegion` extends the libGDX [TextureRegion class](http://libgdx.badlogicgames.com/ci/nightlies/docs/api/com/badlogic/gdx/graphics/g2d/TextureRegion.html).
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
```

For a complete libGDX example, please have a look at [this](http://github.com/Mazatech/amanithsvg-bindings/tree/master/libGDX/gameCards).


---
