---
layout: default
title: "Unity - minimal example"
date: 2018-01-01 08:00:00 +0100
chapter: 2
categories: [tut]
---

# Unity minimal example

The example explained in this chapter is implemented in the [plane scene](https://github.com/Mazatech/amanithsvg-sdk/blob/master/examples/unity/Assets/SVGAssets/Scenes/plane.unity). The example shows a simple plane object with an attached texture, generated from an SVG file. The example has been realized following these steps:

 - Create a new Unity project, AmanithSVG can be used for 2D and 3D projects; in this case we select the 2D setup.

 - Select the Game view, and make sure that "Scale" slider is completely on the left (i.e. `scale = 1x`); switch back to the Scene view. 

    | &nbsp; |
    | :---: |
    | *Set a unitary scene scale* |
    {:.tbl_images .unity_tut_init}

 - Because the project is a new one, it is required to copy [AmanithSVG binding for Unity](http://github.com/Mazatech/amanithsvg-sdk/tree/master/examples/unity/Assets/SVGAssets) (`Editor`, `Plugins`, `Resources`, `SVGFiles` and `Scripts` folders) inside the new project's `Assets/SVGAssets` folder; so that resources (e.g. config file, fonts, SVG files), native AmanithSVG plugins and its C# interface will be available for the project.

    | &nbsp; |
    | :---: |
    | *The minimal project layout after copying all needed files* |
    {:.tbl_images .unity_tut1_copy_binding}

 - Set a full white `Ambient Color` light (menu: `Window` → `Rendering` → `Lighting` → `Environment` → `Environment Lighting`), in order to display SVG files with their original colors (in older Unity version the option was accessible through the `Window` → `Rendering` → `Lighting Settings` menu).

    | &nbsp; |
    | :---: |
    | *Ensure a pure white ambient light* |
    {:.tbl_images .unity_tut1_ambient_light}

 - Create a plane object (menu: `GameObject` → `3D Object` → `Plane`) and make sure that the main camera is pointing it. To do so, place the plane object at `(0, 0, 0)` and the camera object at `(0, 10, 0)` with a rotation of `(90, 0, -180)`.

    | &nbsp; |
    | :---: |
    | *The plane gameobject* |
    {:.tbl_images .unity_tut1_plane_create}

 - Select the plane object and attach the "SVG Texture Behaviour" script (menu `Component` → `Add`, then `Scripts subsection`). This script is really simple, it shows a possible usage of AmanithSVG library. In the detail the script:

    - Creates a drawing surface with the same dimensions

    - Creates and load the SVG document

    - Renders the SVG document to the drawing surface

    - Creates the texture with the specified dimensions

    - Copies the drawing surface pixels to the texture

    - Destroys SVG document and drawing surface

    - Returns the created texture

 - Find the `girl.svg` file (located in `Assets/SVGAssets/SVGFiles`) and drag&drop it from the Project window to the `SVG File` property field of the script. Let other properties unchanged to their default values (`512 x 512` pixels texture dimensions, white background clear color).

 - Click play and see the result.

    | &nbsp; |
    | :---: |
    | *This is the result after pressing Play* |
    {:.tbl_images .unity_tut1_play}

AmanithSVG binding for Unity also includes another script called `SVGTextureAtlasBehaviour`; in order to test it, stop the current execution and perform the following steps:

 - Select the plane and remove the attached `SVGTextureBehaviour` component

 - Add the `SVGTextureAtlasBehaviour` script component: this script allows you to specify four different SVG files to be rendered on a single texture

 - Drag&drop four SVG files from the Project window to `SVG File` `1,2,3,4` property fields of the script.

 - As before, click play and see the result in the Game window.

    | &nbsp; |
    | :---: |
    | *Four SVG files rendered on a single texture* |
    {:.tbl_images .unity_tut1_four_svg}

## Credits

Thanks to "Ror362" for its great girl svg. Original source: [https://www.deviantart.com/ror362/art/Yayoi-iM-S-397886689](https://www.deviantart.com/ror362/art/Yayoi-iM-S-397886689)

---
