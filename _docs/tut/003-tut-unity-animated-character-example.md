---
layout: default
title: "Unity - animated character"
date: 2018-01-01 08:00:00 +0100
chapter: 3
categories: [tut]
---

# Unity animated character example

The example explained in this chapter is implemented in the [orc scene](https://github.com/Mazatech/amanithsvg-sdk/blob/master/examples/unity/Assets/SVGAssets/Scenes/orc.unity). The example shows a simple 2D character (the orc), walking on a (potentially scrollable) background. Both orc and background are native SVG files.

The orc asset is an SVG file where each body part is a separate first level group (`<g>` tag), each group has a well defined name (id attribute). This setup will allow to render each body part as a separate animable sprite. AmanithSVG atlas generator will perform an optimized sprite packing routine in the editor, and later on the target device (at runtime).

| &nbsp; |
| :---: |
| *Adobe Illustrator allows to show and edit groups* |
{:.tbl_images .unity_tut2_ai_orc}

Now we can proceed in the following way:

 - Create a new Unity project, choosing a 2D setup

 - Select the Game view, and make sure that "Scale" slider is completely on the left (i.e. `scale = 1x`); switch back to the Scene view.

    | &nbsp; |
    | :---: |
    | *Set a unitary scene scale* |
    {:.tbl_images .unity_tut_init}

 - Because the project is a new one, it is required to copy [AmanithSVG binding for Unity](https://github.com/Mazatech/amanithsvg-bindings/tree/master/Unity/Assets/SVGAssets) (`Editor`, `Plugins` and `Scripts` folders) inside the new project's `Assets` folder; so the native AmanithSVG libraries and its C# interface will be available for the project. Copy the `Anim` folder too (it contains two simple "idle" and "walking" animation assets), inside the project's Assets folder.

 - Copy the [orc.svg](https://github.com/Mazatech/amanithsvg-sdk/blob/master/examples/unity/Assets/SVGAssets/SVGFiles/orc.svg.txt) and [background.svg](https://github.com/Mazatech/amanithsvg-sdk/blob/master/examples/unity/Assets/SVGAssets/SVGFiles/background.svg.txt) files in the project's `Assets` folder. If needed, such files will be renamed automatically by [SVGRenamerImporter]({{site.url}}/docs/binding/004-unity.html#svgrenamerimporter) script, adding an additional `.txt` extension (if not already included), so that Unity can recognize it as a [TextAsset](https://docs.unity3d.com/ScriptReference/TextAsset.html).

    | &nbsp; |
    | :---: |
    | *The Orc project layout after copying all needed files* |
    {:.tbl_images .unity_tut2_copy_binding}

 - The new scene should have been already populated with an orthographic camera; make sure that camera is positioned at `(0, 0, -10)`

 - Create the sprite that we will use as background (menu `GameObject` → `2D Object` → `Sprite`). We rename the created object as `Background`, we position it at `(0, 0, 0)` and we set the `Order in Layer` value of the `SpriteRenderer` component to `-1`, so the background sprite will always be behind all other sprites. Now we can attach a `SVGBackgroundBehaviour` script to it (menu `Component` → `Add`, then `Scripts` subsection), then set its fields:

    - drag&drop the background.svg file to the `SVG file` property

    - ensure that `Sliced` checkbox is unchecked

    - set scale adaption to `Vertical`, because we want the background to cover the whole device screen height, making the horizontal direction (potentially) scrollable

    - set the `Size` property to `540`, that is the original `background.svg` vertical size (i.e. `height` attribute)

    | &nbsp; |
    | :---: |
    | *The background SVG placed on the scene* |
    {:.tbl_images .unity_tut2_background}

 - As you can see, the background still does not completely cover (vertically) the whole screen, but this is something that we're going to solve soon: first we must however create our orc character.

 - Create an `SVGAtlas` atlas generator (menu `Assets` → `SVGAssets` → `Create SVG sprites atlas`), that will be used to create and pack all the sprites relative to the orc body parts. We rename the created asset in `orcAtlas` just to avoid confusion. Then we can proceed with its settings:

    - drag&drop the orc.svg file to the region labeled with `Drag & drop SVG assets here` and check the `Separate groups` option

    - set reference width and height to `960 x 540` (the dimensions of the background): at design time, we want to see if the orc character size is "compatible" with the background

    - set device test width and height to `960 x 540`, so in the editor we can simulate a device with a resolution equal to the background; setting a device test resolution equal to the reference resolution has the effect to generate all the orc sprites at their original dimension, as specified in the orc.svg file

    - Set scale adaption to vertical, for the same reason that we discussed for the background

    - Click the `Update` button

 - At this point, the texture atlas and the sprites assets relative to orc character parts have been generated, and made available in the editor. Please note that each sprite starts with a pivot point equal to `[0.5, 0.5]`, that means "the center of sprite".

    | &nbsp; |
    | :---: |
    | *The generated orc sprite assets* |
    {:.tbl_images .unity_tut2_orc_sprites}

- Now we can instantiate the whole orc (i.e. all its parts) by clicking the `Instantiate` button present in the row where the orc.svg file has been dropped. In this way, previously generated sprites assets will be instantiated in the scene.

    | &nbsp; |
    | :---: |
    | *Orc has been instantiated with just one click* |
    {:.tbl_images .unity_tut2_orc_instantiated}

Please note that each instantiated sprite:

 - has an attached [SVGSpriteLoaderBehaviour]({{site.url}}/docs/binding/004-unity.html#svgspriteloaderbehaviour) script component, that will take care to regenerate the sprite on the device, at runtime, with the correct scale

 - has the `Order in Layer` value of the `SpriteRenderer` component automatically set, in order to respect the correct z-order induced by the sequence in which groups appear within the orc.svg file

In order to animate each body part like a real character, we must setup:

 - the correct hierarchy of all instantiated sprites (in the `Hierarchy` section on the left, simply drag&drop each child object under its father): we want the body part to be the root of our hierarchy

    | &nbsp; |
    | :---: |
    | *Orc hierarchy, body is the root* |
    {:.tbl_images .unity_tut2_orc_hierarchy}

 - the pivot points of each sprite: to do this, use the pivot editor made available by the `orcAtlas` object

    | &nbsp; |
    | :---: |
    | *The pivot editor window* |
    {:.tbl_images .unity_tut2_sprites_pivot}

| Sprite name               | Pivot (x) | Pivot (y) |
| :------------------------ | --------- | --------- |
| `orc_head`                |  `0.55`   |  `0.40`   |
| `orc_body`                |  `0.50`   |  `0.50`   |
| `orc_dx_x5F_arm_x5F_down` |  `0.50`   |  `0.80`   |
| `orc_dx_x5F_arm_x5F_up`   |  `0.60`   |  `0.70`   |
| `orc_dx_x5F_leg_x5F_down` |  `0.50`   |  `0.80`   |
| `orc_dx_x5F_leg_x5F_up`   |  `0.50`   |  `0.70`   |
| `orc_sx_x5F_arm_x5F_down` |  `0.50`   |  `0.85`   |
| `orc_sx_x5F_arm_x5F_up`   |  `0.45`   |  `0.70`   |
| `orc_sx_x5F_leg_x5F_down` |  `0.40`   |  `0.80`   |
| `orc_sx_x5F_leg_x5F_up`   |  `0.50`   |  `0.70`   |
{:.rwd-table .rwd-table-orcPivots}

Because we are going to move the orc (actually the body part, and consequently the whole children hierarchy) programmatically by code, we must select the `orc_body` object and uncheck the `Update transform` option of the `SVGSpriteLoaderBehaviour` component.

| &nbsp; |
| :---: |
| *"Update transform" must be unchecked for the body part* |
{:.tbl_images .unity_tut2_orc_body_update_transform}

Now the scene is almost complete, so we can save it.

| &nbsp; |
| :---: |
| *The scene is now complete* |
{:.tbl_images .unity_tut2_scene_almost_complete}

If we click the Play button, we see that:

 - orc sprites are resized according to the `Game` view, but the character position is not consistent respect to the background

 - background sprite is not resized

 - the main camera viewing volume is not covering the exact height of the background

| &nbsp; |
| :---: |
| *Playing the scene* |
{:.tbl_images .unity_tut2_scene_play}

The reason is because at design time, in the editor, all sprites and their positions are valid for a (test) device resolution equal to `960 x 540`. So, we want to:

 - make the camera viewing volume (actually the vertical size) to cover the whole screen height: this means to set the `Camera.orthographicSize` properly

 - send a resize event to the background sprite: this means to set its `SVGBackgroundBehaviour.Size` property

 - send a resize event to the orc character: this means to call the orc body sprite (actually its `SVGSpriteLoaderBehaviour` component) `UpdateSprite` function, specifying to update children too

In order to accomplish these tasks, we add a [SVGCameraBehaviour]({{site.url}}/docs/binding/004-unity.html#svgcamerabehaviour) component to the main camera (menu `Component` → `Add`, then `Scripts` subsection): this script, at each monobehaviour `Update` call, checks for a screen resolution change and, if this event occurs, first it sets the `Camera.orthographicSize` properly, then it informs all its listeners through the `OnResize` event.

| &nbsp; |
| :---: |
| *The SVGCameraBehaviour script detects if screen resolution has changed* |
{:.tbl_images .unity_tut2_camera_behaviour}

At this point, we add a script to the `orc_body` sprite (lets call it `OrcCharacterBehaviour`), that has the following input fields:

 - the scene camera, so we can implement and register the `OnResize` event handler
 
 - the background sprite, so we can set its `Size` property within the `OnResize` event handler

Because we are going to resize (i.e. generate at runtime) both background and orc sprites by C# code, before to implement the `OrcCharacterBehaviour`, we must:

 - select the background sprite object and uncheck the `Generate on Start()` option

 - select all the sprites that form the orc and uncheck the `Resize on Start()` option

Then we can select the `orc_body` sprite and implement the `OrcCharacterBehaviour` script as follow (menu `Component` → `Add` → `New script`):

```c#
using UnityEngine;

public class OrcCharacterBehaviour : MonoBehaviour {

    private float BackgroundWalkingLine()
    {
        // walking line is located at ~12% of the background half height, in world coordinates
        return (Background != null) ? (-Background.WorldHeight * 0.5f) * 0.12f : 0.0f;
    }

    private void ResetOrcPos()
    {
        // move the orc at the walking line
        transform.position = new Vector3(0, BackgroundWalkingLine(), 0);
    }

    private void ResizeBackground(int newScreenWidth, int newScreenHeight)
    {
        // we want to cover the whole screen
        Pair<SVGBackgroundScaleType, int> scaleData = Background.CoverFullScreen(newScreenWidth, newScreenHeight);
        Background.ScaleAdaption = scaleData.First;
        Background.Size = scaleData.Second;
        Background.UpdateBackground(false);
    }

    private void ResizeOrcCharacter(int backgroundWidth, int backgroundHeight)
    {
        // get the orc (body) sprite loader
        SVGSpriteLoaderBehaviour spriteLoader = gameObject.GetComponent<SVGSpriteLoaderBehaviour>();
        // update/regenerate all orc sprites; NB: we want to size the orc according to
        // the background sprite (actually the background height)
        spriteLoader.UpdateSprite(true, backgroundWidth, backgroundHeight);
    }

    private void OnResize(int newScreenWidth, int newScreenHeight)
    {
        // render the background so that it covers the whole screen
        ResizeBackground(newScreenWidth, newScreenHeight);
        // update/regenerate all orc sprites according to the background dimensions
        ResizeOrcCharacter((int)Background.PixelWidth, (int)Background.PixelHeight);
        // move the orc at the world origin
        ResetOrcPos();
    }

    // Use this for initialization
    void Start()
    {
        // register ourself for receiving resize events
        Camera.OnResize += OnResize;
        // now fire a resize event by hand
        Camera.Resize(true);
    }

    // the scene camera
    public SVGCameraBehaviour Camera;
    // the background gameobject
    public SVGBackgroundBehaviour Background;
}
```

Before to play the scene, we must set `OrcCharacterBehaviour` properties; so we must drag&drop:

 - The `Main Camera` gameobject to the `Camera` property

 - The `Background` gameobject to the `Background` property

| &nbsp; |
| :---: |
| *The OrcCharacterBehaviour script, attached to the body sprite* |
{:.tbl_images .unity_tut2_orc_behaviour}

Now if we click play, we see that the camera viewing volume is covering the whole screen, the background sprite is resized according to the screen dimensions and the orc character has the same vertical proportion respect to the background! The next step is to move the orc along the horizontal axis, and enable the camera to follow it and scroll the background. To accomplish these tasks, the `OrcCharacterBehaviour` script must implement a function that derives the camera position according to the orc position:

```c#
private Vector3 CameraPosCalc(Vector3 orcPos)
{
    // set the camera according to the orc position
    Vector3 cameraPos = new Vector3(orcPos.x, 0, -10);
    float cameraWorldLeft = cameraPos.x - (Camera.WorldWidth / 2);
    float cameraWorldRight = cameraPos.x + (Camera.WorldWidth / 2);
    // make sure the camera won't go outside the background
    if (cameraWorldLeft < Background.WorldLeft)
    {
        cameraPos.x += Background.WorldLeft - cameraWorldLeft;
    }
    else
    if (cameraWorldRight > this.Background.WorldRight)
    {
        cameraPos.x -= cameraWorldRight - Background.WorldRight;
    }
    return cameraPos;
}

private void CameraPosAssign(Vector3 orcPos)
{
    // set the camera according to the orc position
    Camera.transform.position = (Camera.PixelWidth > Background.PixelWidth) ? new Vector3(0, 0, -10) : CameraPosCalc(orcPos);
}
```

and two functions to move the orc left or right, changing its body world position (the y coordinate will be fixed to the walking line); such functions will be called in the [LateUpdate](https://docs.unity3d.com/ScriptReference/MonoBehaviour.LateUpdate.html) routine, if the user has pressed the mouse button:

```c#
private void Move(Vector3 delta)
{
    // move the orc
    Vector3 orcPos = transform.position + delta;
    // get the orc (body) sprite loader
    SpriteRenderer spriteRenderer = gameObject.GetComponent<SpriteRenderer>();
    float orcBodyWidth = spriteRenderer.sprite.bounds.size.x;
    // orc body pivot is located at 50% of the whole orc sprite, so we can calculate bounds easily
    float orcWorldLeft = orcPos.x - (orcBodyWidth / 2);
    float orcWorldRight = orcPos.x + (orcBodyWidth / 2);
    // make sure the orc won't go outside the background
    if (orcWorldLeft < Background.WorldLeft)
    {
        orcPos.x += (Background.WorldLeft - orcWorldLeft);
    }
    else
    if (orcWorldRight > Background.WorldRight)
    {
        orcPos.x -= (orcWorldRight - Background.WorldRight);
    }
    // update the orc position
    transform.position = orcPos;
    // set the camera according to the orc position
    this.CameraPosAssign(orcPos);
}

private void MoveLeft()
{
    // flip the orc horizontally
    this.transform.localScale = new Vector3(-1, this.transform.localScale.y, this.transform.localScale.z);
    this.Move(new Vector3(-WALKING_SPEED, 0, 0));
}

private void MoveRight()
{
    this.transform.localScale = new Vector3(1, this.transform.localScale.y, this.transform.localScale.z);
    this.Move(new Vector3(WALKING_SPEED, 0, 0));
}

void LateUpdate()
{
    if (Input.GetButton("Fire1"))
    {
        Vector3 worldMousePos = Camera.GetComponent<Camera>().ScreenToWorldPoint(Input.mousePosition);
        if (worldMousePos.x > transform.position.x)
        {
            this.MoveRight();
        }
        else
        if (worldMousePos.x < transform.position.x)
        {
            this.MoveLeft();
        }
    }
}

// the walking speed, in world coordinates
private const float WALKING_SPEED = 0.04f;
```

As you can see, all the code (orc and camera movement) is based on world coordinates. The source code of the final script can be found [here](https://github.com/Mazatech/amanithsvg-sdk/blob/master/examples/unity/Assets/SVGAssets/Scripts/OrcScene/OrcBehaviour.cs).

In order to complete the example, we must animate the orc; we can use the "idle" and "walking" animations that we have already prepared in the `Assets/Anim/OrcScene` folder:

 - select the `orc_body` gameobject
 
 - add an `Animator` component (menu `Component` → `Miscellaneous` → `Animator`)

 - drag&drop the `orc_body` controller from the `Assets/Anim/OrcScene` to the Animator's `Controller` field

| &nbsp; |
| :---: |
| *Animation attached to the orc body* |
{:.tbl_images .unity_tut2_orc_animate}

Save the scene and click play and see the animated orc, starting with the "idle" animation; by pressing the mouse (or tapping the device screen), the orc will walk towards right or left.

Now you can have fun experimenting with it!

## Credits

 - Thanks to Chris Hildenbrand for its awesome orc svg. Original source: [http://2dgameartforprogrammers.blogspot.com](https://2dgameartforprogrammers.blogspot.com)
 - Thanks to Casper Tang Veje ("adcoon") for its beautiful background svg. Original source: [https://www.deviantart.com/adcoon](https://www.deviantart.com/adcoon)

---
