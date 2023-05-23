---
layout: default
title: "Unity"
date: 2018-01-01 08:00:00 +0100
chapter: 4
categories: [binding]
---

# AmanithSVG native plugins for Unity, requirements and limitations

AmanithSVG native plugins for Unity differ from other AmanithSVG standalone builds because they implement some specific methods to speed up the upload of pixels to textures as much as possible. These specific methods make direct use of native graphics APIs, which are different for each supported platform. In the detail, AmanithSVG native plugins for Unity support:

- Windows platform (x86, x86_64), Direct3D 11
- MacOS X platform (Apple Silicon, x64), Metal
- Linux platform (x86, x86_64), OpenGL
- iOS X platform (Apple Silicon, x64), Metal
- Android platform (ARMv7, ARM64, x86, x86_64), OpenGL ES

**Vulkan graphics API is not supported by AmanithSVG native plugins for Unity**.

To make sure that AmanithSVG for Unity can continue to be used even in the presence of unsupported native graphics APIs, several fallbacks have been implemented using standard (but slower) Unity calls ([GetRawTextureData](https://docs.unity3d.com/ScriptReference/Texture2D.GetRawTextureData.html), [GetUnsafeBufferPointerWithoutChecks](https://docs.unity3d.com/ScriptReference/Unity.Collections.LowLevel.Unsafe.NativeArrayUnsafeUtility.GetUnsafeBufferPointerWithoutChecks.html), [LoadRawTextureData](https://docs.unity3d.com/ScriptReference/Texture2D.LoadRawTextureData.html)).
The use of fast texture upload via native graphics APIs, or the fallbacks mechanism via Unity legacy functions, is controlled by the `Fast upload` flag of [SVGTextureBehaviour](#svgtexturebehaviour), [SVGBackgroundBehaviour](#svgbackgroundbehaviour), [SVGAtlas](#svgatlas) classes.

# Unity binding

[Unity](https://unity3d.com) is a cross-platform game engine that can be used to create both three-dimensional and two-dimensional games as well as simulations for its many platforms. Unity engine offers a primary scripting API in C#, for both the Unity editor in the form of plugins, and games themselves, as well as drag and drop functionality.

So the AmanithSVG binding for Unity consists in a set of C# classes and editor plugins in order to expose AmanithSVG functionalities in a simple way; the binding extends the higher layer C# binding already discussed [here]({{site.url}}/docs/binding/001-csharp.html). In particular four new classes have been introduced:

 - [SVGSurfaceUnity](#svgsurfaceunity)
 - [SVGResourceUnity](#svgresourceunity)
 - [SVGAssetsConfigUnity](#svgassetsconfigunity)
 - [SVGAssetsUnity](#svgassetsunity)

Then, using these new classes, the Unity binding exposes additional classes that integrate AmanithSVG features with Unity specific mechanisms (e.g. monobehaviour, sprites, texture atlas and so on). Here's the list of implemented Unity-specific classes:

 - [SVGTextureBehaviour](#svgtexturebehaviour)
 - [SVGBackgroundBehaviour](#svgbackgroundbehaviour)
 - [SVGCameraBehaviour](#svgcamerabehaviour)
 - [SVGAtlas](#svgatlas)
 - [SVGSpriteLoaderBehaviour](#svgspriteloaderbehaviour)
 - [SVGUIAtlas](#svguiatlas)
 - [SVGUISpriteLoaderBehaviour](#svguispriteloaderbehaviour)
 - [SVGCanvasBehaviour](#svgcanvasbehaviour)

**When working with Unity engine, it is mandatory to use `SVGAssetsUnity` class and not the basic `SVGAssets` one**.

---

## SVGSurfaceUnity

[This class](https://github.com/Mazatech/amanithsvg-sdk/blob/master/examples/unity/Assets/SVGAssets/Scripts/SVGAssets/SVGAssetsUnity.cs) extends [SVGSurface]({{site.url}}/docs/binding/001-csharp.html#svgsurface) by providing methods to generate Unity textures out of surface pixels.

```c#
/*
    Create a 2D texture compatible with the drawing surface.

    NB: textures passed to Copy and CopyAndDestroy must be created
    through this function.
*/
Texture2D CreateCompatibleTexture(bool bilinearFilter,
                                  bool wrapRepeat,
                                  HideFlags hideFlags);


/*
    Copy drawing surface content into the specified Unity texture.
    NB: the given texture must have been created by the CreateCompatibleTexture
    method.

    It returns SVGError.None if the operation was completed successfully, else
    an error code.
*/
SVGError Copy(Texture2D texture);

/*
    Copy drawing surface content into the specified Unity texture, then destroy
    the native drawing surface. The SVGSurfaceUnity instance is not destroyed, but
    its native AmanithSVG counterpart it will. The result will be that every
    called method will fail silently.

    In order to ensure the maximum speed, the copy process will use native GPU
    platform-specific methods:

    - UpdateSubresource (Direct3D 11)
    - glTexSubImage2D (OpenGL and OpenGL ES)
    - replaceRegion (Metal)

    NB: the given texture must have been created by the CreateCompatibleTexture
    method.

    It returns SVGError.None if the operation was completed successfully, else
    an error code.
*/
SVGError CopyAndDestroy(Texture2D texture);
```

An `SVGSurfaceUnity` can be created by using the `SVGAssetsUnity.CreateSurface` function, specifying its dimensions in pixels.

---

## SVGResourceUnity

[This class](https://github.com/Mazatech/amanithsvg-sdk/blob/master/examples/unity/Assets/SVGAssets/Scripts/SVGAssets/SVGAssetsUnity.cs) extends [SVGResource]({{site.url}}/docs/binding/001-csharp.html#svgresource) by providing the mandatory `GetBytes` method to get an in-memory representation of the underlying binary TTF / OTF / WOFF / JPEG / PNG file. The constructor takes a [TextAsset](https://docs.unity3d.com/ScriptReference/TextAsset.html) and sets the given unique string identifier, the type (font or image) and hints. The access to the underlying binary data is implemented by using the `TextAsset.bytes` property.

---

## SVGAssetsConfigUnity

[This class](https://github.com/Mazatech/amanithsvg-sdk/blob/master/examples/unity/Assets/SVGAssets/Scripts/SVGAssets/SVGAssetsUnity.cs) extends [SVGAssetsConfig]({{site.url}}/docs/binding/001-csharp.html#svgassetsconfig) by implementing an internal list of `SVGResourceUnity` resources. Items within this list can be added, moved and deleted. Most importantly this class implements the mandatory `ResourcesCount` and `GetResource` methods from `SVGAssetsConfig` class, in order to get the resources.

---

## SVGAssetsUnity

[SVGAssetsUnity](https://github.com/Mazatech/amanithsvg-sdk/blob/master/examples/unity/Assets/SVGAssets/Scripts/SVGAssets/SVGAssetsUnity.cs) is a static class through which `SVGDocument`, `SVGSurface`, `SVGSurfaceUnity` and `SVGPacker` instances can be created. It extends `SVGAssets` class by implementing additional methods specific for Unity:

```c#
/* Get screen resolution width, in pixels. */
uint ScreenWidth;

/* Get screen resolution height, in pixels. */
uint ScreenHeight;

/* Get screen dpi. */
float ScreenDpi;

/*
    Create a drawing surface, specifying its dimensions in pixels.

    Supplied dimensions should be positive numbers greater than zero, else
    a null instance will be returned.
*/
SVGSurface CreateSurface(uint width,
                         uint height);

/*
    Create and load an SVG document, specifying the whole XML string.
    If supplied XML string is null or empty, a null instance will be returned.
*/
SVGDocument CreateDocument(string xmlText);

/*
    Create an SVG packer, specifying a scale factor.

    Every collected SVG document/element will be packed into rectangular bins,
    whose dimensions won't exceed the specified 'maxTexturesDimension' in pixels.

    If true, 'pow2Textures' will force bins to have power-of-two dimensions.
    Each rectangle will be separated from the others by the specified 'border' in pixels.

    The specified 'scale' factor will be applied to all collected SVG documents/elements,
    in order to realize resolution-independent atlases.
*/
SVGPacker CreatePacker(float scale,
                       uint maxTexturesDimension,
                       uint border,
                       bool pow2Textures);

/*
    Create an SVG packer, specifying a scale factor.

    Every collected SVG document/element will be packed into rectangular bins.

    Each rectangle will be separated from the others by the specified 'border' in pixels.

    The specified 'scale' factor will be applied to all collected SVG documents/elements,
    in order to realize resolution-independent atlases.

    NB: maximum dimension of textures is auto-detected.
*/
SVGPacker CreatePacker(float scale,
                       uint border);

/*
    Create a sprite out of the given texture.
    The sprite will use the specified rectangular section of the
    texture, and it will have the provided pivot.
*/
Sprite CreateSprite(Texture2D texture,
                    Rect rect,
                    Vector2 pivot);

```

**When working with Unity engine, it is mandatory to use `SVGAssetsUnity` class and not the basic `SVGAssets` one**.

---

## SVGTextureBehaviour

[This monobehaviour](https://github.com/Mazatech/amanithsvg-sdk/blob/master/examples/unity/Assets/SVGAssets/Scripts/SVGAssets/SVGTextureBehaviour.cs) script performs a single SVG file rendering on a [Texture2D](https://docs.unity3d.com/ScriptReference/Texture2D.html).
It takes in input the SVG file (as a [TextAsset](https://docs.unity3d.com/ScriptReference/TextAsset.html)), the texture dimensions and a clear color. Such color will be used to clear the surface before to start the rendering. The rendering is performed in the [Start](https://docs.unity3d.com/ScriptReference/MonoBehaviour.Start.html) function, that assigns the generated texture to the `renderer.sharedMaterial.mainTexture` field of the associated [GameObject](https://docs.unity3d.com/ScriptReference/GameObject.html). The `Fast upload` flag enables the native update of the texture instead to use the (slower) legacy [LoadRawTextureData](https://docs.unity3d.com/ScriptReference/Texture2D.LoadRawTextureData.html) + [Apply](https://docs.unity3d.com/ScriptReference/Texture2D.Apply.html) method.

| &nbsp; |
| :---: |
| *Editor mask for the SVGTextureBehaviour component* |
{:.tbl_images .unitytut_SVGTextureBehaviour}

If the `Fast upload` checkbox is selected, AmanithSVG native plugins for Unity will update the Unity texture using the following methods:

 - [UpdateSubresource](https://docs.microsoft.com/en-us/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-updatesubresource) (Direct3D 11)

 - [glTexSubImage2D](https://www.khronos.org/registry/OpenGL-Refpages/es2.0/xhtml/glTexSubImage2D.xml) (OpenGL and OpenGL ES)

 - [replaceRegion](https://developer.apple.com/documentation/metal/mtltexture/1515464-replaceregion?language=objc) (Metal)

You can see the usage of `SVGTextureBehaviour` script by opening the [plane scene](https://github.com/Mazatech/amanithsvg-sdk/blob/master/examples/unity/Assets/SVGAssets/Scenes/plane.unity).

| &nbsp; |
| :---: |
| *The "plane" scene* |
{:.tbl_images .unitytut_unity_plane_scene}

---

## SVGBackgroundBehaviour

[This monobehaviour](https://github.com/Mazatech/amanithsvg-sdk/blob/master/examples/unity/Assets/SVGAssets/Scripts/SVGAssets/SVGBackgroundBehaviour.cs) script performs the rendering of a single SVG file on a `Texture2D`, and creates a new sprite out of it (covering the whole texture). The sprite is then assigned to the [SpriteRenderer](https://docs.unity3d.com/ScriptReference/SpriteRenderer.html) component. As for the [SVGTextureBehaviour](#svgtexturebehaviour) script, it is possible to specify a clear color and the `Fast upload` flag too.

The script has two operational modes: sliced or not sliced (according to the relative checkbox).

When sliced, the script takes two additional parameters, `Width` and `Height`: at runtime, the generated texture/sprite will be sized at `Width` x `Height` exactly, in a way to preserve the SVG aspect ratio and at the same time to cover the largest dimension between `Width` and `Height` (SVG will be aligned to the center).

| &nbsp; |
| :---: |
| *Editor mask for the SVGBackgroundBehaviour component* |
{:.tbl_images .unitytut_unity_background_behaviour_mask}

Sliced mode is useful to generate static backgrounds that must cover exactly the whole device screen; you can have a look at the [game scene](https://github.com/Mazatech/amanithsvg-sdk/blob/master/examples/unity/Assets/SVGAssets/Scenes/game.unity) for a such usage case.

| &nbsp; |
| :---: |
| *The "game" scene* |
{:.tbl_images .unitytut_unity_game_scene}

In the non-sliced mode, the `SVGBackgroundBehaviour` script takes two different additional parameters, `Scale adaption` and `Size`. The first one could be `Horizontal` or `Vertical`, while the second one is expressed in pixels.
The rendering is performed keeping the (horizontal or vertical) dimension equal to the specified `Size` value, and calculates the other texture dimension preserving the original SVG aspect ratio.
So the final generated texture will have a size of:

 - `Size` x (`Size` * `SVG aspect ratio`), if `Scale adaption == Horizontal`; or

 - (`Size` * `SVG aspect ratio`) x `Size`, if `Scale adaption == Vertical`

The non-sliced mode is useful to generate static "scrollable" backgrounds (i.e. camera viewport smaller than the generated texture); you can have a look at the [orc scene](https://github.com/Mazatech/amanithsvg-sdk/blob/master/examples/unity/Assets/SVGAssets/Scenes/orc.unity) for a such usage case.

| &nbsp; |
| :---: |
| *The "orc" scene* |
{:.tbl_images .unitytut_unity_orc_scene}

NB: in order to work properly, the `SVGBackgroundBehaviour` script must be assigned to a `GameObject` with a `SpriteRenderer` component already attached (e.g. you can create it by using the `GameObject` → `2D Object` → `Sprite` menu).

---

## SVGCameraBehaviour

In order to work properly, [SVGCameraBehaviour](https://github.com/Mazatech/amanithsvg-sdk/blob/master/examples/unity/Assets/SVGAssets/Scripts/SVGAssets/SVGCameraBehaviour.cs) script must be attached to an orthographic [Camera](https://docs.unity3d.com/ScriptReference/Camera.html).
The script ensures that both camera [aspect ratio](https://docs.unity3d.com/ScriptReference/Camera-aspect.html) and [orthographic size](https://docs.unity3d.com/ScriptReference/Camera-orthographicSize.html) match the current screen resolution. In addition, the script is in charge of communicating a possible change of the screen resolution (e.g. when a mobile device is rotated or a window resized) to all `OnResize` listeners.

| &nbsp; |
| :---: |
| *Editor mask for the SVGCameraBehaviour component* |
{:.tbl_images .unitytut_SVGCameraBehaviour}

Both [orc scene](https://github.com/Mazatech/amanithsvg-sdk/blob/master/examples/unity/Assets/SVGAssets/Scenes/orc.unity) and [game scene](https://github.com/Mazatech/amanithsvg-sdk/blob/master/examples/unity/Assets/SVGAssets/Scenes/game.unity) use the `SVGCameraBehaviour` script.

---

## SVGAtlas

[This ScriptableObject](https://github.com/Mazatech/amanithsvg-sdk/blob/master/examples/unity/Assets/SVGAssets/Scripts/SVGAssets/SVGAtlas.cs) allows the generation of sprites from different SVG files, packing the generated sprites into one or more textures, according to the specified parameters.

| &nbsp; |
| :---: |
| *Editor mask for the SVGAtlas scriptable object* |
{:.tbl_images .unitytut_SVGAtlas_scriptable}

Each SVG file can be drag&dropped in the relative section (the region labeled with "Drag & drop SVG assets here"). Dragged items can be moved within the list, changing their order: this will induce an automatic z-order (i.e. [Order in Layer](https://docs.unity3d.com/ScriptReference/Renderer-sortingOrder.html) value of `SpriteRenderer` components) of generated sprites. In the detail, sprites will be ordered in a way such that the one on the top of the list will be placed behind all others.

For each SVG entry, it is possible to specify if the altas generator must render it as a whole single sprite ("Separate groups" option unchecked), or if first level groups (`<g>`tags) must be rendered as separate sprites ("Separate groups" option checked). This option will allow the animation of each group.

| &nbsp; |
| :---: |
| *Example of generated textures and sprites* |
{:.tbl_images .unitytut_generated_textures_sprites}

Being vector based, SVG can be freely scaled without visual quality loss. In order to give the user an effective way to control the scaling factor, `SVGAtlas` makes available the following parameters:

 - Reference width, height: it's the resolution at which SVG files would be rendered at the native dimensions, specified within the SVG file itself (outermost `<svg>` element, attributes `width` and `height`). It is really important that all used SVG files contain such attributes, because if they are not available, AmanithSVG will render each sprite filling a whole texture.

 - Device test (simulated) width, height: it's the resolution that will be simulated to generate and test sprites in the editor. These settings allow the user to test different device resolutions in advance.

 - Screen match mode: it defines which device dimension will control the scaling factor. The match mode is defined by the [SVGScalerMatchMode](https://github.com/Mazatech/amanithsvg-sdk/blob/master/examples/unity/Assets/SVGAssets/Scripts/SVGAssets/SVGAssets.cs) enum type, and it's an extension of the Unity [ScreenMatchMode](https://docs.unity3d.com/ScriptReference/UIElements.PanelScreenMatchMode.html).

 - Offset scale: an additional scale factor that will be applied to all SVG files.

The general formula to calculate the scaling factor is:

`Scl` = (`Device dimension` / `Reference dimension`) * `OffsetScale`

where device dimension is the one chosen by the screen match mode parameter.

For example, lets have:

 - an SVG file with `100 x 150` dimensions (i.e. `<svg width="100px" height="150px">`)

 - a reference resolution set to `1024 x 768`

 - screen match mode set to `Vertical`

 - offset scale set to `1`

On a `640 x 480` device resolution, the sprite will be rendered at `62 x 94` pixels, because scaling factor is `0.625 (480 / 768)`.
On a `1536 x 2048` device resolution, the sprite will be rendered at `266 x 400` pixels, because scaling factor is `2.667 (2048 / 768)`.

For the complete formula, please have a look at the `ScaleFactorCalc` method of [SVGScaler](https://github.com/Mazatech/amanithsvg-sdk/blob/master/examples/unity/Assets/SVGAssets/Scripts/SVGAssets/SVGAssets.cs) class.

Two additional script parameters control some aspects relative to the generated textures:

 - Force pow2 textures: if checked, this parameters will force the atlas generator to produce textures whose dimensions are power-of-two values.

 - Max textures dimension: the maximum dimension allowed during the textures atlas generation. Please make sure that such value won't exceed the real device capability.

The last two script parameters control how many pixels will separate each sprite from any other (`Sprite padding`) and the color that is used to clear the surface before to start the rendering (`Clear color`).
The `Fast upload` flag has the same meaning described for the [SVGTextureBehaviour](#svgtexturebehaviour) class.

---

## SVGSpriteLoaderBehaviour

[This monobehaviour](https://github.com/Mazatech/amanithsvg-sdk/blob/master/examples/unity/Assets/SVGAssets/Scripts/SVGAssets/SVGSpriteLoaderBehaviour.cs) script takes care to render the associated sprite at runtime on the device, according to its original [SVGAtlas](#svgatlas) generator settings.

| &nbsp; |
| :---: |
| *Editor mask for the SVGSpriteLoaderBehaviour component* |
{:.tbl_images .unitytut_SVGSpriteLoaderBehaviour}

Editable parameters consist in:

 - `Resize on Start`: if checked (default value), the sprite will be regenerated at the [Start](https://docs.unity3d.com/ScriptReference/MonoBehaviour.Start.html) of monobehaviour, else the user must update the sprite programmatically calling the public `UpdateSprite` function.

 - `Update transform`: if checked (default value), at every monobehaviour [LateUpdate](https://docs.unity3d.com/ScriptReference/MonoBehaviour.LateUpdate.html) call, the sprite local position will be fixed according to its current dimensions. This will ensure that animated child sprites will be in the correct position relative to their father (just the position is relevant, rotation and scale do not need to be fixed). If the sprite is moved programmatically by code (e.g. a root body part of a character), this option must be unchecked.

`Atlas` and `Sprite` properties refer to the `SVGAtlas` object that has generated this sprite.

`SVGSpriteLoaderBehaviour` component is attached automatically to those sprites instantiated through the [SVGAtlasEditor](#svgatlaseditor) editor mask.

---

## SVGUIAtlas

[SVGUIAtlas](https://github.com/Mazatech/amanithsvg-sdk/blob/master/examples/unity/Assets/SVGAssets/Scripts/SVGAssets/SVGUIAtlas.cs) is really similar to [SVGAtlas](#svgatlas), with the only exception that all parameters that determine the scale factor for SVG contents (e.g. reference resolution, screen match mode, and so on) are provided by a [CanvasScaler](https://docs.unity3d.com/Documentation/Manual/script-CanvasScaler.html) component.
`SVGUIAtlas` objects cannot be instantiated as a standalone asset (as opposed to `SVGAtlas`), but they are strictly attached to [SVGCanvasBehaviour](#svgcanvasbehaviour) components: each `SVGCanvasBehaviour` instance contains a single `SVGUIAtlas` instance.
Because `SVGCanvasBehaviour` components can be attached to [Canvas](https://docs.unity3d.com/ScriptReference/Canvas.html) only, it follows that `SVGUIAtlas` instances are in a 1-1 relation to canvas instances.

---

## SVGUISpriteLoaderBehaviour

[This monobehaviour](https://github.com/Mazatech/amanithsvg-sdk/blob/master/examples/unity/Assets/SVGAssets/Scripts/SVGAssets/SVGUISpriteLoaderBehaviour.cs) script takes care to render the associated UI sprite at runtime on the device, according to its original [SVGUIAtlas](#svguiatlas) generator settings.

| &nbsp; |
| :---: |
| *Editor mask for the SVGUISpriteLoaderBehaviour component* |
{:.tbl_images .unitytut_SVGUISpriteLoaderBehaviour}


Editable parameters consist in:

 - `Resize on Start`: if checked (default value), the UI sprite will be regenerated at the [Start](https://docs.unity3d.com/ScriptReference/MonoBehaviour.Start.html) of monobehaviour, else the user must update the sprite programmatically calling the public `UpdateSprite` function.

 - `Sprite` property refers to the sprite generated by the relative `SVGUIAtlas` object.

The `Edit` button is a shortcut to the pivot and borders editor that would still be reachable by selecting the original [SVGCanvasBehaviour](#svgcanvasbehaviour) component.

`SVGUISpriteLoaderBehaviour` component is attached automatically to those UI sprites instantiated through the [SVGCanvasEditor](#svgcanvaseditor) editor mask.

---

## SVGCanvasBehaviour

[This monobehaviour](https://github.com/Mazatech/amanithsvg-sdk/blob/master/examples/unity/Assets/SVGAssets/Scripts/SVGAssets/SVGCameraBehaviour.cs) script can be attached to [Canvas](https://docs.unity3d.com/ScriptReference/Canvas.html) components only, and it is in charge of storing an instance of [SVGUIAtlas](#svguiatlas).
The main role of `SVGCanvasBehaviour` is to communicate the [canvas scale factor](https://docs.unity3d.com/ScriptReference/Canvas-scaleFactor.html) to the maintained `SVGUIAtlas` instance.

| &nbsp; |
| :---: |
| *Editor mask for the SVGCanvasBehaviour component* |
{:.tbl_images .unitytut_SVGCanvasBehaviour}

---

# Editor scripts

To make sure that the use of AmanithSVG is as simple and convenient as possible from the Unity editor, a set of editor scripts have been implemented:

 - [SVGAssetsConfigProvider](#svgassetsconfigprovider)
 - [SVGBackgroundEditor](#svgbackgroundeditor)
 - [SVGCameraEditor](#svgcameraeditor)
 - [SVGAtlasEditor](#svgatlaseditor)
 - [SVGPivotEditor](#svgpivoteditor)
 - [SVGSpriteLoaderEditor](#svgspriteloadereditor)
 - [SVGAtlasSelector](#svgatlasselector)
 - [SVGSpriteSelector](#svgspriteselector)
 - [SVGCanvasEditor](#svgcanvaseditor)
 - [SVGUISpriteLoaderEditor](#svguispriteloadereditor)
 - [SVGRenamerImporter](#svgrenamerimporter)

---

## SVGAssetsConfigProvider

[This editor script](https://github.com/Mazatech/amanithsvg-sdk/blob/master/examples/unity/Assets/SVGAssets/Editor/SVGAssets/SVGAssetsConfigEditor.cs) implements a `SettingsProvider` for the editing of [SVGAssetsConfigUnity](#svgassetsconfigunity). Is is possible to configure user-agent language, curves quality and log facilities. In addition it is possible to drag&drop resource files (fonts and images) and edit their parameters. The editing window is available throught the `Edit` → `Project Settings` → `SVGAssets` menu.

---

## SVGBackgroundEditor

[This editor script](https://github.com/Mazatech/amanithsvg-sdk/blob/master/examples/unity/Assets/SVGAssets/Editor/SVGAssets/SVGBackgroundEditor.cs) implements the inspector mask for the editing of [SVGBackgroundBehaviour](#svgbackgroundbehaviour) components. The layout allows to edit all `SVGBackgroundBehaviour` parameters, taking care to present only those needed ones according to the current operational mode (sliced or not-sliced).

| &nbsp; |
| :---: |
| *Editor mask for the SVGBackgroundBehaviour component* |
{:.tbl_images .unitytut_unity_background_behaviour_mask}

---

## SVGCameraEditor

[This editor script](https://github.com/Mazatech/amanithsvg-sdk/blob/master/examples/unity/Assets/SVGAssets/Editor/SVGAssets/SVGCameraEditor.cs) implements the inspector mask for [SVGCameraBehaviour](#svgcamerabehaviour) components. The layout simply displays the camera viewport size, both in pixels and world units.

| &nbsp; |
| :---: |
| *Editor mask for the SVGCameraBehaviour component* |
{:.tbl_images .unitytut_SVGCameraBehaviour}

---

## SVGAtlasEditor

[This editor script](https://github.com/Mazatech/amanithsvg-sdk/blob/master/examples/unity/Assets/SVGAssets/Editor/SVGAssets/SVGAtlasEditor.cs) implements the inspector mask for the editing of [SVGAtlas](#svgatlas) assets. The base class `SVGBasicAtlasEditor` (from which `SVGAtlasEditor` is derived) also implements a `PostProcessBuild` static class that will take care of some aspects relative to the building phase. In particular, it ensures that during the building process all generated atlases won't be included in the final package (actually they are substituted by a dummy 1x1 texture), because they will be regenerated at runtime on the device. This will reduce the build size, showing the advantage of using SVG files instead of bitmaps.

---

## SVGPivotEditor

[This editor script](https://github.com/Mazatech/amanithsvg-sdk/blob/master/examples/unity/Assets/SVGAssets/Editor/SVGAssets/SVGPivotEditor.cs) allows the user to edit the pivot point of generates sprites. The pivot can be moved by simply clicking the mouse on the desired position or by changing values by hand. It is desirable to setup pivot points before instantiating and/or animating sprites.

| &nbsp; |
| :---: |
| *The pivot editor window* |
{:.tbl_images .unitytut_pivot_editor}

---

## SVGSpriteLoaderEditor

[This editor script](https://github.com/Mazatech/amanithsvg-sdk/blob/master/examples/unity/Assets/SVGAssets/Editor/SVGAssets/SVGSpriteLoaderEditor.cs) implements the inspector mask for the editing of [SVGSpriteLoaderBehaviour](#svgspriteloaderbehaviour) components. The layout allows to edit (actually to check/uncheck) the `Resize on start` and `Update transform` properties. In addition the `Atlas` and `Sprite` buttons allow to show and/or select the sprite reference: in particular, such two buttons make use of [SVGAtlasSelector](#svgatlasselector) and [SVGSpriteSelector](#svgspriteselector) editor scripts.

| &nbsp; |
| :---: |
| *Editor mask for the SVGSpriteLoaderBehaviour component* |
{:.tbl_images .unitytut_SVGSpriteLoaderBehaviour}

---

## SVGAtlasSelector

[This editor script](https://github.com/Mazatech/amanithsvg-sdk/blob/master/examples/unity/Assets/SVGAssets/Editor/SVGAssets/SVGAtlasSelector.cs) implements a [ScriptableWizard](https://docs.unity3d.com/ScriptReference/ScriptableWizard.html) that allows the (visual) selection of `SVGAtlas` assets.

| &nbsp; |
| :---: |
| *Select SVGAtlas objects wihtin the project* |
{:.tbl_images .unitytut_unity_atlas_selector}

---

## SVGSpriteSelector

[This editor script](https://github.com/Mazatech/amanithsvg-sdk/blob/master/examples/unity/Assets/SVGAssets/Editor/SVGAssets/SVGSpriteSelector.cs) implements a [ScriptableWizard](https://docs.unity3d.com/ScriptReference/ScriptableWizard.html) that allows the (visual) selection of a sprite asset generated by `SVGAtlas` objects.

| &nbsp; |
| :---: |
| *Select a sprite belonging to an SVGAtlas object* |
{:.tbl_images .unitytut_unity_sprite_selector}

---

## SVGCanvasEditor

[This editor script](https://github.com/Mazatech/amanithsvg-sdk/blob/master/examples/unity/Assets/SVGAssets/Editor/SVGAssets/SVGCanvasEditor.cs) implements the inspector mask for the editing of [SVGCanvasBehaviour](#svgcanvasbehaviour) components. In the detail the script displays the canvas scale factor and the underlying [SVGUIAtlas](#svguiatlas) object.
The base class `SVGBasicAtlasEditor` (from which `SVGCanvasEditor` is derived) also implements a `PostProcessBuild` static class that will take care of some aspects relative to the building phase. In particular, it ensures that during the building process all generated atlases won't be included in the final package (actually they are substituted by a dummy `1x1` texture), because they will be regenerated at runtime on the device. This will reduce the build size, showing the advantage of using SVG files instead of bitmaps.

| &nbsp; |
| :---: |
| *Editor mask for the SVGCanvasBehaviour component* |
{:.tbl_images .unitytut_SVGCanvasBehaviour}

---

## SVGUISpriteLoaderEditor

[This editor script](https://github.com/Mazatech/amanithsvg-sdk/blob/master/examples/unity/Assets/SVGAssets/Editor/SVGAssets/SVGUISpriteLoaderEditor.cs) implements the inspector mask for the editing of [SVGUISpriteLoaderBehaviour](#svguispriteloaderbehaviour) components. The layout allows to edit (actually to check/uncheck) the `Resize on Start` property. In addition the `Sprite` button allows to show and/or select the sprite reference: in particular, such button makes use of [SVGSpriteSelector](#svgspriteselector) editor script.
The `Edit` button allows the modification of sprite borders and pivot.

| &nbsp; |
| :---: |
| *Editor mask for the SVGUISpriteLoaderBehaviour component* |
{:.tbl_images .unitytut_SVGUISpriteLoaderBehaviour}

It is desirable to setup pivot points before instantiating UI sprites.

---

## SVGRenamerImporter

[This script implements](https://github.com/Mazatech/amanithsvg-sdk/blob/master/examples/unity/Assets/SVGAssets/Editor/SVGAssets/SVGRenamerImporter.cs) an asset post-processor ([AssetPostprocessor](https://docs.unity3d.com/ScriptReference/AssetPostprocessor.html) class). Its task consists in changing the extension of new asset files from `.svg` to `.svg.txt`, so Unity can recognize those files as [text assets](https://docs.unity3d.com/Manual/class-TextAsset.html). The same kind of task is also performed on `.ttf`, `.otf`, `.woff`, `.jpg` and `.png` files: in this case they will be renamed by adding a `.bytes` suffix.

---

# Unity binding FAQ

## AmanithSVG native plugins for Android

AmanithSVG native plugins for Android must be placed in `Assets/SVGAssets/Plugins/Android/libs` folder. According to the target architecture, a different `libAmanithSVG.so` file is present in a different subdirectory. Be sure to follow this [guide](https://docs.unity3d.com/Manual/android-native-plugins-import.html) in order to import AmanithSVG native plugins for Android correctly; in the detail:

 * select each `libAmanithSVG.so` file present in `Assets/SVGAssets/Plugins/Android/libs` subfolders and view it in the Inspector
 * deselect `Any platform` option, if checked
 * select `Include Platforms` --> `Android` option
 * in the `Platform settings` section, set `CPU` to:
     * `ARMv7` for `armeabi-v7a/libAmanithSVG.so`
     * `ARM64` for `arm64-v8a/libAmanithSVG.so`
     * `X86` for `x86/libAmanithSVG.so`
     * `X86_64` for `x86_64/libAmanithSVG.so`

**The official [AmanithSVG SDK](https://github.com/Mazatech/amanithsvg-sdk)** already includes the plugins set correctly.

---

## AmanithSVG on iOS doesn't render the SVG(s) and XCode does not show any messages about it. How to fix it?

Please double check that the folder `Assets/SVGAssets/Plugins/iOS` of your project contains both the static library `libAmanithSVG.a` and [loader.m](https://github.com/Mazatech/amanithsvg-sdk/blob/master/examples/unity/Assets/SVGAssets/Plugins/iOS/loader.m)

---
