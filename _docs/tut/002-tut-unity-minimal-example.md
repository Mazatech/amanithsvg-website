---
layout: default
title: "Unity - minimal example"
date: 2018-01-01 08:00:00 +0100
chapter: 2
categories: [tut]
---

# Unity minimal example

The example explained in this chapter is contained in the [plane scene](http://github.com/Mazatech/amanithsvg-bindings/blob/master/Unity/Assets/SVGAssets/Scenes/plane.unity). The example shows a simple plane object with an attached texture, generated from an SVG file. The example has been realized following these steps:

 - Create a new Unity project, AmanithSVG can be used for 2D and 3D projects; in this case we select the 2D setup.

 - Select the Game view, and make sure that "Scale" slider is completely on the left (i.e. `scale = 1x`); switch back to the Scene view. 

    | ![Set a unitary scene scale]({{site.url}}/assets/images/unity_tut1_init.png) |
    | :---: |
    | *Set a unitary scene scale* |

 - Because the project is a new one, it is required to copy [AmanithSVG binding for Unity](http://github.com/Mazatech/amanithsvg-bindings/tree/master/Unity/Assets/SVGAssets) (Editor, Plugins and Scripts folders) inside the new project's `Assets` folder; so the native AmanithSVG libraries and its C# interface will be available for the project.

    | ![The project layout after copying all needed files]({{site.url}}/assets/images/unity_tut1_copy_binding.png) | 
    | :---: |
    | *The project layout after copying all needed files* |

 - Set a full white `Ambient Color` light (menu: `Window` → `Rendering` → `Lighting Settings`), in order to display SVG files with their original colors.

    | ![Ensure a pure white ambient light]({{site.url}}/assets/images/unity_tut1_ambient_light.png) | 
    | :---: |
    | *Ensure a pure white ambient light* |

 - Create a plane object (menu: `GameObject` → `3D Object` → `Plane`) and make sure that the main camera is pointing it. To do so, place the plane object at `(0, 0, 0)` and the camera object at `(0, 10, 0)` with a rotation of `(90, 0, -180)`.

    | ![The plane gameobject]({{site.url}}/assets/images/unity_tut1_plane_create.png) | 
    | :---: |
    | *The plane gameobject* |

 - Copy an SVG file (e.g. [girl.svg](http://github.com/Mazatech/amanithsvg-bindings/blob/master/Unity/Assets/SVGAssets/SVGFiles/girl.svg.txt) within the `Assets` directory. If needed, such file will be renamed automatically by [SVGRenamerImporter]({{site.url}}/docs/binding/004-unity.html#svgrenamerimporter) script, adding an additional `.txt` extension (if not already included), so that Unity can recognize it as a [TextAsset](http://docs.unity3d.com/ScriptReference/TextAsset.html).

 - Select the plane object and attach the "SVG Texture Behaviour" script (menu `Component` → `Add`, then `Scripts subsection`). This script is really simple, it shows a possible usage of AmanithSVG library. In the detail the script:
    
    - Creates a drawing surface with the same dimensions
    
    - Creates and load the SVG document
    
    - Renders the SVG document to the drawing surface
    
    - Creates the texture with the specified dimensions
    
    - Copies the drawing surface pixels to the texture
    
    - Destroys SVG document and drawing surface
    
    - Returns the created texture

 - Drag&drop the girl SVG file from the Project window to the `SVG File` property field of the script. Let other properties unchanged to their default values (`512 x 512` pixels texture dimensions, white background clear color).

 - Click play and see the result.
 
    | ![This is the result after pressing Play]({{site.url}}/assets/images/unity_tut1_play.png) | 
    | :---: |
    | *This is the result after pressing Play* |

AmanithSVG binding for Unity also includes another script called `SVGTextureAtlasBehaviour`; in order to test it, stop the current execution and perform the following steps:

 - Select the plane and remove the attached `SVGTextureBehaviour` component

 - Add the `SVGTextureAtlasBehaviour` script component: this script allows you to specify four different SVG files to be rendered on a single texture

 - Drag&drop four SVG files from the Project window to `SVG File` `1,2,3,4` property fields of the script.

 - As before, click play and see the result in the Game window.

    | ![Four SVG files rendered on a single texture]({{site.url}}/assets/images/unity_tut1_four_svg.png) | 
    | :---: |
    | *Four SVG files rendered on a single texture* |


---
