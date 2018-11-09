---
layout: default
title: "LibGDX - game example"
date: 2018-01-01 08:00:00 +0100
chapter: 1
categories: [tut]
---

# LibGDX memory game example

In this tutorial, we will create a simple prototype of a memory-like game, using [libGDX](http://libgdx.badlogicgames.com) and the relative [AmanithSVG binding API]({{site.url}}/docs/binding/003-libgdx.html).
We will create the project following the [official guide](http://libgdx.badlogicgames.com/documentation/gettingstarted/Creating%20Projects.html).


---

## The setup

First, lets create the project structure:

 - Make sure all the required libGDX [prerequirements](http://libgdx.badlogicgames.com/documentation/gettingstarted/Setting%20Up.html) are met. Especially those concerning the [command line](http://libgdx.badlogicgames.com/documentation/gettingstarted/Setting%20Up.html#command-line), because we will use this approach to build the whole project 

 - Download libGDX project setup tool [gdx-setup.jar](http://libgdx.badlogicgames.com/nightlies/dist/gdx-setup.jar)

 - Open up your command line tool, go to the download folder and run:
   
   ```
   java -jar gdx-setup.jar
   ```

 - Fill all required fields as show in the following image
   
    | ![Project setup using the command line]({{site.url}}/assets/images/libgdx_tut1_setup_1.png) |
    | :---: |
    | *Project setup using the command line* |

Now the project folders structure should look like the following:

| ![The project folders structure]({{site.url}}/assets/images/libgdx_tut1_setup_2.png) |
| :---: |
| *The project folders structure* |

The next step is to include the [AmanithSVG binding for libGDX]({{site.url}}/docs/binding/003-libgdx.html) to the project:

 - copy the [com.mazatech.svgt](http://github.com/Mazatech/amanithsvg-bindings/tree/master/libGDX/gameCards/core/src/com/mazatech/svgt) sources within the project's `core/src` folder
 
 - copy the [com.mazatech.gdx](http://github.com/Mazatech/amanithsvg-bindings/tree/master/libGDX/gameCards/core/src/com/mazatech/gdx) sources within the project's `core/src` folder

 - edit the `build.gradle` file present within the project main directory:
    
    - add `amanithsvgVersion = '1.9.9'` to the `allprojects` → `ext` section
    
    - add `implementation "com.mazatech.amanithsvg:amanithsvg-gdx:$amanithsvgVersion:natives-desktop"` to the `project(":desktop")` → `dependencies` section
    
    - add `natives "com.mazatech.amanithsvg:amanithsvg-gdx:$amanithsvgVersion:natives-armeabi"` to the `project(":android")` → `dependencies` section
    
    - add `natives "com.mazatech.amanithsvg:amanithsvg-gdx:$amanithsvgVersion:natives-armeabi-v7a"` to the `project(":android")` → `dependencies` section
    
    - add `natives "com.mazatech.amanithsvg:amanithsvg-gdx:$amanithsvgVersion:natives-arm64-v8a"` to the `project(":android")` → `dependencies` section
    
    - add `natives "com.mazatech.amanithsvg:amanithsvg-gdx:$amanithsvgVersion:natives-x86"` to the `project(":android")` → `dependencies` section
    
    - add `natives "com.mazatech.amanithsvg:amanithsvg-gdx:$amanithsvgVersion:natives-x86_64"` to the `project(":android")` → `dependencies` section
    
    - add `implementation "com.mazatech.amanithsvg:amanithsvg-gdx:$amanithsvgVersion:natives-ios"` to the `project(":ios")` → `dependencies` section
    
    | ![AmanithSVG native library dependencies added to build.gradle file]({{site.url}}/assets/images/libgdx_tut1_setup_3.png) |
	| :---: |
	| *AmanithSVG native library dependencies added to build.gradle file* |

After these operations, the `build.gradle` file should look like [this](http://github.com/Mazatech/amanithsvg-bindings/blob/master/libGDX/gameCards/build.gradle) and the folders structure should look like the following:

| ![The project now includes AmanithSVG packages]({{site.url}}/assets/images/libgdx_tut1_setup_4.png) |
| :---: |
| *The project now includes AmanithSVG packages* |

Now we are ready to check the project setup by running, from the project main directory, the command `./gradlew desktop:run` on Linux and MacOS X systems or `gradlew desktop:run` on Windows systems.
If all is ok, you should be able to see the classic libGDX "Hello world"-like window.

| ![Project setup has been completed successfully]({{site.url}}/assets/images/libgdx_tut1_setup_5.png) |
| :---: |
| *Project setup has been completed successfully* |


---

## The basic template

From now until the end we will work on the `MyGdxGame.java` file. First of all we remove the Badlogic Games image logo, and we plug AmanithSVG in. To do so:

 - we import `com.mazatech.svgt` and `com.mazatech.gdx` packages

 - we initialize AmanithSVG library within the [create](http://libgdx.badlogicgames.com/ci/nightlies/docs/api/com/badlogic/gdx/ApplicationAdapter.html#create--) function, by calling `SVGAssets.init()`

 - we release AmanithSVG resources within the [dispose](http://libgdx.badlogicgames.com/ci/nightlies/docs/api/com/badlogic/gdx/ApplicationAdapter.html#dispose--) function, by calling `SVGAssets.dispose()`

The complete basic template of our game will look as follow:

```java
package com.mygdx.game;

// libGDX
import com.badlogic.gdx.ApplicationAdapter;
import com.badlogic.gdx.Gdx;
import com.badlogic.gdx.graphics.GL20;
import com.badlogic.gdx.graphics.g2d.SpriteBatch;
import com.badlogic.gdx.graphics.OrthographicCamera;

// AmanithSVG
import com.mazatech.svgt.*;
import com.mazatech.gdx.*;

public class MyGdxGame extends ApplicationAdapter {

    private SpriteBatch batch;
    private OrthographicCamera camera;
    
    void amanithsvgInfoDisplay() {
        
        String vendor = AmanithSVG.svgtGetString(AmanithSVG.SVGT_VENDOR);
        String version = AmanithSVG.svgtGetString(AmanithSVG.SVGT_VERSION);

        // display some basic information about installed AmanithSVG library
        Gdx.app.log("MyGdxGame", "AmanithSVG vendor = " + vendor);
        Gdx.app.log("MyGdxGame", "AmanithSVG version = " + version);
    }

    @Override
    public void create() {

        // initialize AmanithSVG
        SVGAssets.init();

        // display some basic information about installed AmanithSVG library
        amanithsvgInfoDisplay();

        // create the batch (used by 'render' function)
        batch = new SpriteBatch();

        // setup orthographic camera
        camera = new OrthographicCamera();
        camera.setToOrtho(false, Gdx.graphics.getBackBufferWidth(),
                                 Gdx.graphics.getBackBufferHeight());
        camera.update();

        // setup the clear color just once
        Gdx.gl.glClearColor(1.0f, 1.0f, 1.0f, 1.0f);
    }

    @Override
    public void resize(int width, int height) {
        
        if ((width > 0) && (height > 0)) {
            // update OpenGL viewport
            Gdx.gl.glViewport(0, 0, width, height);
            // update current projection matrix
            camera.setToOrtho(false, width, height);
            camera.update();
        }
    }

    @Override
    public void render() {

        // clear the whole screen
        Gdx.gl.glClear(GL20.GL_COLOR_BUFFER_BIT);
        batch.setProjectionMatrix(camera.combined);
    }
    
    @Override
    public void dispose() {

        batch.dispose();
        // release AmanithSVG resources
        SVGAssets.dispose();
    }
}
```

If you run the project, you will get the following result:

| ![The basic template of our game]({{site.url}}/assets/images/libgdx_tut1_basic_template.png) |
| :---: |
| *The basic template of our game* |


---

## The game background

Now it's time to add some cute SVG backgrounds to our game, so lets start to copy [gameBkg1.svg](http://github.com/Mazatech/amanithsvg-bindings/blob/master/libGDX/gameCards/android/assets/gameBkg1.svg), [gameBkg2.svg](http://github.com/Mazatech/amanithsvg-bindings/blob/master/libGDX/gameCards/android/assets/gameBkg2.svg), [gameBkg3.svg](http://github.com/Mazatech/amanithsvg-bindings/blob/master/libGDX/gameCards/android/assets/gameBkg3.svg), [gameBkg4.svg](http://github.com/Mazatech/amanithsvg-bindings/blob/master/libGDX/gameCards/android/assets/gameBkg4.svg) files to the project's 'android/assets' folder.

We store the [SVGDocument]({{site.url}}/docs/binding/002-java.html#svgdocument) instances relative to the four backgrounds and keep track of the generated background [texture]({{site.url}}/docs/binding/003-libgdx.html#svgtexture):

```java
// SVG background documents
private SVGDocument[] backgroundDocs = { null, null, null, null };
// the actual background texture
private SVGTexture backgroundTexture = null;
```

We load all four SVG backgrounds files at initialization time (`create` function). Please note that we override the default [alignment]({{site.url}}/docs/binding/002-java.html#svgalignment), because we want the background to cover the whole screen.

```java
@Override public void create() {

    // initialize AmanithSVG
    SVGAssets.init();

    ...

    // create backgrounds documents
    backgroundDocs[0] = SVGAssets.createDocument(Gdx.files.internal("gameBkg1.svg"));
    backgroundDocs[1] = SVGAssets.createDocument(Gdx.files.internal("gameBkg2.svg"));
    backgroundDocs[2] = SVGAssets.createDocument(Gdx.files.internal("gameBkg3.svg"));
    backgroundDocs[3] = SVGAssets.createDocument(Gdx.files.internal("gameBkg4.svg"));
    // backgrounds viewBox must cover the whole drawing surface, so we use
    // SVGTMeetOrSlice.Slice (default is SVGTMeetOrSlice.Meet)
    for (int i = 0; i < 4; ++i) {
        backgroundDocs[i].setAspectRatio(new SVGAlignment(SVGTAlign.XMidYMid, 
                                                          SVGTMeetOrSlice.Slice));
    }

    ...
}
```

We resize the background (i.e. we generate the background texture at a new resolution) at each `resize` event:

```java
void generateBackground(int screenWidth, int screenHeight) {

    // destroy previous backgound texture
    if (backgroundTexture != null) {
        backgroundTexture.dispose();
    }

    // generate a new background texture
    backgroundTexture = new SVGTexture(backgroundDocs[0], 
                                       screenWidth, screenHeight,
                                       SVGColor.Clear, false);
}

@Override public void resize(int width, int height) {

    ...

    if ((width > 0) && (height > 0)) {
        // generate background
        generateBackground(width, height);
    }
}
```

We draw the background texture within the `render` function:

```java
@Override public void render() {

    ...

    batch.begin();
    // draw the background (texture, x, y,
    //                      width, height,
    //                      srcX, srcY, srcWidth, srcHeight,
    //                      flipX, flipY)
    batch.draw(backgroundTexture, 0.0f, 0.0f,
               Gdx.graphics.getBackBufferWidth(), Gdx.graphics.getBackBufferHeight(),
               0, 0, backgroundTexture.getWidth(), backgroundTexture.getHeight(),
               false, true);
    batch.end();
}
```

And finally we dispose the four background documents and the background texture when the application is being closed (`dispose` function):

```java
@Override public void dispose() {

    ...

    for (int i = 0; i < 4; ++i) {
        backgroundDocs[i].dispose();
    }
    backgroundTexture.dispose();

    // release AmanithSVG resources
    SVGAssets.dispose();
}
```

| ![Backgrounds used within the game]({{site.url}}/assets/images/libgdx_tut1_four_backgrounds.png) |
| :---: |
| *Backgrounds used within the game* |

---

## The cards

The characters hidden behind the cards are cute animals, here are their name (see full implementation [here](http://github.com/Mazatech/amanithsvg-bindings/blob/master/libGDX/gameCards/core/src/com/mazatech/amanithsvg/gamecards/CardType.java)):

```java
public enum CardType {

    Undefined(0),
    BackSide(1),
    Panda(2),
    Monkey(3),
    Orangutan(4),
    Panther(5),
    Puma(6),
    Leopard(7),
    Lion(8),
    Cougar(9),
    Tiger(10),
    Elephant(11),
    Penguin(12),
    Zebra(13),
    Hen(14),
    Rooster(15),
    Pig(16),
    Dog(17),
    Rabbit(18),
    Owl(19),
    Sheep(20),
    Cat(21),
    Deer(22),
    Donkey(23),
    Cow(24),
    Fox(25);

    // Number of total animal types
    public static int count();

    // Select a random animal
    public static CardType random();

    // Get the next animal in the list
    public CardType next();

    // Map an animal name to the respective enum value (e.g. "puma" -> Puma)
    static CardType fromName(final String name);
}
```

We model a single card as a set of basic attributes (see full implementation [here](http://github.com/Mazatech/amanithsvg-bindings/blob/master/libGDX/gameCards/core/src/com/mazatech/amanithsvg/gamecards/Card.java)):

```java
public class Card {

    public Card() {

        active = true;
        animalType = CardType.BackSide;
        backSide = true;
    }

    // true if card is active (i.e. still part of the current game), else false
    public boolean active;
    // true if card is back side, false if the card is turned
    // (i.e. we can see the animal character)
    public boolean backSide;
    // the animal character associated with this card
    public CardType animalType;
    // card position within the screen, in pixels
    public float x;
    public float y;
    // card dimensions, in pixels
    public float width;
    public float height;
}
```

Animals vector graphics are defined within a single SVG file, where each animal is a single first-level group (a `<g>` element):

```xml
<?xml version="1.0" encoding="utf-8"?>
<!DOCTYPE svg PUBLIC "-//W3C//DTD SVG 1.1//EN" "http://www.w3.org/Graphics/SVG/1.1/DTD/svg11.dtd">
<svg version="1.1"
	 id="Animals"
	 xmlns="http://www.w3.org/2000/svg"
	 xmlns:xlink="http://www.w3.org/1999/xlink"
	 width="768px" height="640px"
	 viewBox="0 0 3072 2560"
	 xml:space="preserve">
    <g id="Panda">...</g>
    <g id="Monkey">...</g>
    <g id="Orangutan">...</g>
    <g id="Panther">...</g>
    <g id="Puma">...</g>
    <g id="Leopard">...</g>
    <g id="Lion">...</g>
    <g id="Cougar">...</g>
    <g id="Tiger">...</g>
    <g id="Elephant">...</g>
    <g id="Penguin">...</g>
    <g id="Zebra">...</g>
    <g id="Hen">...</g>
    <g id="Rooster">...</g>
    <g id="Pig">...</g>
    <g id="Dog">...</g>
    <g id="Rabbit">...</g>
    <g id="Owl">...</g>
    <g id="Sheep">...</g>
    <g id="Cat">...</g>
    <g id="Deer">...</g>
    <g id="Donkey">...</g>
    <g id="Cow">...</g>
    <g id="Fox">...</g>
    <g id="back">...</g>
</svg>
```

| ![animals.svg]({{site.url}}/assets/images/animals.png) |
| :---: |
| *animals.svg* |

Lets start by copying [animals.svg](http://github.com/Mazatech/amanithsvg-bindings/blob/master/libGDX/gameCards/android/assets/animals.svg) to the project's `android/assets` folder.
Now we want to implement a function that generates animals sprites from the given SVG file, taking care of the current screen resolution. This is easy with AmanithSVG, we make use of [SVGTextureAtlasGenerator]({{site.url}}/docs/binding/003-libgdx.html#svgtextureatlasgenerator) class and the [SVGScaler](http://github.com/Mazatech/amanithsvg-bindings/blob/master/Java/com/mazatech/svgt/SVGScaler.java) utility. Along with the animal sprites creation, we want to create a 1-1 [map](http://docs.oracle.com/javase/7/docs/api/java/util/Map.html) between each sprite and the relative `CardType` enum value.

First of all we load `animals.svg` file and create an instance of `SVGTextureAtlasGenerator` within the `create` function:

```java
// the SVG scaler (for animal sprites)
private SVGScaler scaler;
// SVG atlas generator
private SVGTextureAtlasGenerator atlasGen = null;
// associate each animal type the respective texture region
private Map<CardType, SVGTextureAtlasRegion> animalsSprites = null;

@Override public void create() {

    // initialize AmanithSVG
    SVGAssets.init();
    ...

    // create backgrounds documents
    ...

    // the scaler will calculate the correct scaling factor, actual parameters say:
    // "We have created all the SVG files (that we are going to pack in atlas) so
    // that, at 768 x 640 (the 'reference resolution'), they do not need additional
    // scaling (the last passed parameter value 1.0f is the basic scale relative to
    // the 'reference resolution').
    // If the device has a different screen resolution, we want to scale SVG contents
    // depending on the actual width and height (MatchWidthOrHeight), equally
    // important (0.5f)"
    scaler = new SVGScaler(768, 640, SVGScalerMatchMode.MatchWidthOrHeight, 0.5f, 1.0f);

    // scale, maxTexturesDimension (take care of OpenGL and AmanithSVG limitations),
    // border, pow2Textures, dilateEdgesFix, clearColor
    atlasGen = new SVGTexturGame example
------------
In this tutorial, we will create a simple prototype of a memory-like game, using Unity and the relative [AmanithSVG binding API](/docs/binding/004-unity.html).


The setup
---------
First, lets create the project structure:

- Create a new Unity project, choosing a 2D setup

- Select the Game view, and make sure that "Scale" slider is completely on the left (i.e. 'scale = 1x'); switch back to the Scene view.
[ unity_tut3_init.png ](Set a unitary scene scale)

- Because the project is a new one, it is required to copy AmanithSVG binding for Unity ('Editor', 'Plugins' and 'Scripts' folders) inside the new project's 'Assets' folder; so the native AmanithSVG libraries and its C# interface will be available for the project.

- Copy the [animals.svg](http://github.com/Mazatech/amanithsvg-bindings/blob/master/Unity/Assets/SVGAssets/SVGFiles/animals.svg.txt), [gameBkg1.svg](http://github.com/Mazatech/amanithsvg-bindings/blob/master/Unity/Assets/SVGAssets/SVGFiles/gameBkg1.svg.txt), [gameBkg2.svg](http://github.com/Mazatech/amanithsvg-bindings/blob/master/Unity/Assets/SVGAssets/SVGFiles/gameBkg1.svg.txt), [gameBkg3.svg](http://github.com/Mazatech/amanithsvg-bindings/blob/master/Unity/Assets/SVGAssets/SVGFiles/gameBkg1.svg.txt), [gameBkg4.svg](http://github.com/Mazatech/amanithsvg-bindings/blob/master/Unity/Assets/SVGAssets/SVGFiles/gameBkg4.svg.txt) files in the project's 'Assets' folder. If needed, such files will be renamed automatically by [SVGRenamerImporter](/docs/binding/004-unity.html#svgrenamerimporter) script, adding an additional '.txt' extension (if not already included), so that Unity can recognize it as a [TextAsset](http://docs.unity3d.com/ScriptReference/TextAsset.html).

- The new scene should have been already populated with an orthographic camera; make sure that camera is positioned at (0, 0, -10)

[ unity_tut3_copy_binding.png](The project layout after copying all needed files)


The game background
-------------------
Like we did in the [previous tutorial](/docs/tut/003-tut-unity-animated-character-example), in order to setup a background that covers the whole device screen, we proceed with the following steps:

- we add a [SVGCameraBehaviour](/docs/binding/004-unity.html#svgcamerabehaviour) component to the main camera (menu 'Component → Add', then 'Scripts' subsection): this script, at each monobehaviour 'Update' call, checks for a screen resolution change and, if this event occurs, first it sets the 'Camera.orthographicSize' properly, then it informs all its listeners through the 'OnResize' event.

- we create the sprite that we will use as background (menu 'GameObject → 2D Object → Sprite'). We rename the created object as 'Background', we position it at '(0, 0, 0)' and we set the 'Order in Layer' value of the 'SpriteRenderer' component to '-1', so the background sprite will always be behind all other sprites. Now we attach a 'SVGBackgroundBehaviour' script to it (menu 'Component → Add', then 'Scripts' subsection), then set its fields:
    - drag&drop one of the four backgrounds (e.g. 'gameBkg1.svg') file to the 'SVG file' property
    - check the 'Sliced' checkbox, because we don't want to generate a potentially scrollable background (see the relative [documentation](/docs/binding/004-unity.html#svgbackgroundbehaviour))
    - uncheck the 'Generate on Start()' checkbox (we will perform the generation at runtime, when the camera 'OnResize' event will inform us about a screen resolution change)
    - set 'Width' and 'Height' values to something valid (please set '768 x 640', the reason will be clear in the next paragraph), just to have something visible in the editor

- we create an "empty" 'GameObject' (menu 'GameObject → Create Empty') that will represent our main entry point (i.e. the game state and logic), we rename the created object as 'Game', then we add a script to it (menu 'Component → Add → New script'); lets call it 'GameExampleBehaviour' (you can see the final full implementation [here](http://github.com/Mazatech/amanithsvg-bindings/blob/master/Unity/Assets/SVGAssets/Scripts/GameScene/GameBehaviour.cs)).

[ unity_tut3_template.png ](The basic template of our project)

Here is 'GameExampleBehaviour' basic C# layout (the code is self-explanatory):

using System;
using UnityEngine;

public class GameExampleBehaviour : MonoBehaviour {

    private void ResizeBackground(int newScreenWidth, int newScreenHeight)
    {
        // we want to cover the whole screen
        this.Background.SlicedWidth = newScreenWidth;
        this.Background.SlicedHeight = newScreenHeight;

        // generate the background texture at the desired resolution
        this.Background.UpdateBackground(true);
    }

    private void OnResize(int newScreenWidth, int newScreenHeight)
    {
        // resize the background
        this.ResizeBackground(newScreenWidth, newScreenHeight);
    }

    private void StartNewGame()
    {
        // destroy current background texture
        this.Background.DestroyAll(true);

        // assign a new SVG file
        this.Background.SVGFile = this.BackgroundFiles[this.m_BackgroundIndex % this.BackgroundFiles.Length];

        // advance for the next background SVG
        this.m_BackgroundIndex++;
    }

    // Use this for initialization
    void Start()
    {

        // start with the first background
        this.m_BackgroundIndex = 0;

        // start a new game
        this.StartNewGame();

        // register ourself for receiving resize events
        this.Camera.OnResize += this.OnResize;
        // now fire a resize event by hand
        this.Camera.Resize(true);
    }
    
    // the main camera, used to intercept screen resize events
    public SVGCameraBehaviour Camera;
    // the game background
    public SVGBackgroundBehaviour Background;
    // array of usable SVG backgrounds
    public TextAsset[] BackgroundFiles;

    // the current background (i.e. the index within the BackgroundFiles array)
    [NonSerialized]
    private int m_BackgroundIndex;
}

The last thing left to do is to assign the public fields, by drag&drop:

- the camera gameobject to the 'Camera' field
- the background gameobject to the 'Background' field
- the four background SVG (gameBkg1.svg, gameBkg2.svg, gameBkg3.svg, gameBkg4.svg) to the 'BackgroundFiles' field

[ unity_tut3_background.png ](The game starts to take form: camera and the background sprite...done!)

Now if we click play, we see that the camera viewing volume is covering the whole screen and the background sprite is resized according to the screen dimensions.


The cards
---------
The characters hidden behind the cards are cute animals and we model a single card instance as a set of basic attributes (see full implementation [here](http://github.com/Mazatech/amanithsvg-bindings/blob/master/Unity/Assets/SVGAssets/Scripts/GameScene/GameCardBehaviour.cs)). Lets start by creating a new C# script (menu 'Assets → Create → C# Script') and call it 'GameExampleCardBehaviour':

using System;
using UnityEngine;
#if UNITY_EDITOR
    using UnityEditor;
#endif

public enum GameExampleCardType
{
    Undefined  =  -2,
    BackSide   =  -1,
    Panda      =   0,
    Monkey     =   1,
    Orangutan  =   2,
    Panther    =   3,
    Puma       =   4,
    Leopard    =   5,
    Lion       =   6,
    Cougar     =   7,
    Tiger      =   8,
    Elephant   =   9,
    Penguin    =  10,
    Zebra      =  11,
    Hen        =  12,
    Rooster    =  13,
    Pig        =  14,
    Dog        =  15,
    Rabbit     =  16,
    Owl        =  17,
    Sheep      =  18,
    Cat        =  19,
    Deer       =  20,
    Donkey     =  21,
    Cow        =  22,
    Fox        =  23
};

[ExecuteInEditMode]
public class GameExampleCardBehaviour : MonoBehaviour {

    // given an animal type, it returns the relative sprite name
    public static string AnimalSpriteName(GameExampleCardType animalType)
    {
        switch (animalType)
        {
            case GameExampleCardType.BackSide:
                return("animals_back");
            case GameExampleCardType.Panda:
                return("animals_Panda");
            case GameExampleCardType.Monkey:
                return("animals_Monkey");
            case GameExampleCardType.Orangutan:
                return("animals_Orangutan");
            case GameExampleCardType.Panther:
                return("animals_Panther");
            case GameExampleCardType.Puma:
                return("animals_Puma");
            case GameExampleCardType.Leopard:
                return("animals_Leopard");
            case GameExampleCardType.Lion:
                return("animals_Lion");
            case GameExampleCardType.Cougar:
                return("animals_Cougar");
            case GameExampleCardType.Tiger:
                return("animals_Tiger");
            case GameExampleCardType.Elephant:
                return("animals_Elephant");
            case GameExampleCardType.Penguin:
                return("animals_Penguin");
            case GameExampleCardType.Zebra:
                return("animals_Zebra");
            case GameExampleCardType.Hen:
                return("animals_Hen");
            case GameExampleCardType.Rooster:
                return("animals_Rooster");
            case GameExampleCardType.Pig:
                return("animals_Pig");
            case GameExampleCardType.Dog:
                return("animals_Dog");
            case GameExampleCardType.Rabbit:
                return("animals_Rabbit");
            case GameExampleCardType.Owl:
                return("animals_Owl");
            case GameExampleCardType.Sheep:
                return("animals_Sheep");
            case GameExampleCardType.Cat:
                return("animals_Cat");
            case GameExampleCardType.Deer:
                return("animals_Deer");
            case GameExampleCardType.Donkey:
                return("animals_Donkey");
            case GameExampleCardType.Cow:
                return("animals_Cow");
            case GameExampleCardType.Fox:
                return("animals_Fox");
            default:
                return("");
        }
    }

    // Number of total animal types
    public static int AnimalsCount() {

        return (GameExampleCardType.Fox - GameExampleCardType.Panda) + 1;
    }

    // Select a random animal
    public static GameExampleCardType RandomAnimal() {

        int v = (int)(UnityEngine.Random.value * (float)AnimalsCount()) + (int)GameExampleCardType.Panda;
        return (GameExampleCardType)v;
    }

    // Get the next animal in the list
    public static GameExampleCardType NextAnimal(GameExampleCardType current) {

        int next = (((int)current + 1) % AnimalsCount()) + (int)CardType.Panda;
        return (GameExampleCardType)next;
    }

#if UNITY_EDITOR
    // Reset is called when the user hits the Reset button in the Inspector's context menu or when adding the component the first time.
    // This function is only called in editor mode. Reset is most commonly used to give good default values in the inspector.
    void Reset()
    {
        this.Active = true;
        this.BackSide = true;
        this.AnimalType = GameExampleCardType.Undefined;
        this.Game = null;
    }
#endif

    // true if card is active (i.e. still part of the current game), else false
    public bool Active;
    // true if card is back side, false if the card is turned
    // (i.e. we can see the animal character)
    public bool BackSide;
    // the animal character associated with this card
    public GameExampleCardType AnimalType;
    // a link to the game main script
    public GameExampleBehaviour Game;
}

Animals vector graphics are defined within a single SVG file ([animals.svg](http://github.com/Mazatech/amanithsvg-bindings/blob/master/Unity/Assets/SVGAssets/SVGFiles/animals.svg.txt)), where each animal is a single first-level group (a '<g>' element):

[ TO DO copiare l'xml del file animals.svg come nel tutorial di libGDX ]
[ animals.png ](animals.svg)

As you can see from the 'animals.svg' header, it has been designed for a 768 x 640 resolution (the same that we have chosen, not by chance, for the background).
Now we want to implement a function that generates animals sprites from the given SVG file, taking care of the current screen resolution. This is easy with AmanithSVG, we make use of [SVGAtlas](docs/binding/004-unity.html#svgatlas) class. We create an atlas generator (menu 'Assets → SVGAssets → Create SVG sprites atlas'), that will be used to create and pack all the sprites relative to the animals. We rename the created asset as 'animalsAtlas' just to avoid confusion. Then we can proceed with its settings:

- drag&drop the animals.svg file to the region labeled with 'Drag & drop SVG assets' here and check the 'Separate groups' option

- set reference width and height to 768 x 640 (the dimensions of the background): so the animals sprites size will be "compatible" with the background

- set device test width and height to 768 x 640, so in the editor we can simulate a device with a resolution equal to the background

- leave the 'Screen match mode' to the defalt 'Match Width Or Height'

- leave the 'Match' slider to the default '0.5' value

Click the Update button and you'll see the generated animals sprites. Note that at the 768 x 640 'reference resolution', each animal sprite will have a dimension of 128 x 128: it will ALWAYS guarantee that we can easily place them on a 4 x 3 grid (landscape layout) or on a 3 x 4 grid (portrait layout), REGARDLESS OF SCREEN RESOLUTION.

[ unity_tut3_animals_atlas.png ](Animals sprites have been generated from animals.svg)

Now we instantiate a single animal sprite, and we model a game card on it:

- choose an animal (for example the cat) and click on the relative 'Instantiate' button
- rename the created gameobject to 'Card00'
- uncheck both 'Resize on Start()' and 'Update transform' checkboxes (because we want to resize and position the sprite programmatically, by C# code at runtime)
- add a 'GameExampleCardBehaviour' component (menu 'Component → Add', then 'Scripts' subsection)
- drag&drop the 'Game' gameobject to the 'Game' property (so the card can access to its game)
- add a 'Box Collider 2D' component (menu 'Component → Physics 2D → Box Collider 2D'), because we want to intercept mouse/touch events

[ unity_tut3_card_instantiated.png ](We have instantiated our first card!)

The game will include a set of 12 cards, so all we have to do is to clone 11 times the newly created card gameobject (it doesn't matter where the clones are placed in the editor); in addition we will link the 'GameExampleBehaviour' component (that is our main entry point) to the 12 cards by defining an internal array of 'GameExampleCardBehaviour':

// array of cards
public GameExampleCardBehaviour[] Cards;
// the atlas used to generate animals sprite
public SVGAtlas Atlas;

[ unity_tut3_cards_cloned.png ] (Now game has supervision on backgrounds SVG, card objects and animals sprites)

The next step is to generate animals sprites at runtime, at each camera 'OnResize' event; on the same occasion we have to rearrange the cards on the screen to take account of the new resolution. We split such two functionalities in different functions, within the 'GameExampleBehaviour' component:

public Sprite UpdateCardSprite(GameExampleCardBehaviour card)
{
    GameExampleCardType cardType = card.BackSide ? GameExampleCardType.BackSide : card.AnimalType;
    // get the sprite, given its name
    SVGRuntimeSprite data = this.Atlas.GetSpriteByName(GameExampleCardBehaviour.AnimalSpriteName(cardType));
    // keep updated the SVGSpriteLoaderBehaviour component too
    SVGSpriteLoaderBehaviour loader = card.gameObject.GetComponent<SVGSpriteLoaderBehaviour>();
   
    card.gameObject.GetComponent<SpriteRenderer>().sprite = data.Sprite;
    loader.SpriteReference = data.SpriteReference;

    return data.Sprite;
}

private void UpdateCardsSprites()
{
    // assign the new sprites and update colliders
    for (int i = 0; i < this.Cards.Length; ++i)
    {
        Sprite sprite = this.UpdateCardSprite(this.Cards[i]);
        this.Cards[i].GetComponent<BoxCollider2D>().size = sprite.bounds.size;
    }
}

private void ResizeCards(int newScreenWidth, int newScreenHeight)
{
    float scale;

    // update card sprites according to the current screen resolution
    if (this.Atlas.UpdateRuntimeSprites(newScreenWidth, newScreenHeight, out scale))
    {
        // assign the new sprites and update colliders
        this.UpdateCardsSprites();
    }
}

private void DisposeCards()
{
    int[] cardsIndexes;
    int slotsPerRow, slotsPerColumn;
    SVGRuntimeSprite data = this.Atlas.GetSpriteByName(GameExampleCardBehaviour.AnimalSpriteName(GameExampleCardType.BackSide));
    float cardWidth = data.Sprite.bounds.size.x;
    float cardHeight = data.Sprite.bounds.size.y;
    float worldWidth = this.Camera.WorldWidth;
    float worldHeight = this.Camera.WorldHeight;

    if (worldWidth <= worldHeight) {
        // number of card slots in each dimension
        slotsPerRow = 3;
        slotsPerColumn = 4;
        cardsIndexes = CARDS_INDEXES_PORTRAIT;
    }
    else {
        // number of card slots in each dimension
        slotsPerRow = 4;
        slotsPerColumn = 3;
        cardsIndexes = CARDS_INDEXES_LANDSCAPE;
    }

    // 5% border
    float ofsX = worldWidth * 0.05f;
    float ofsY = worldHeight * 0.05f;
    float horizSeparator = ((worldWidth - (slotsPerRow * cardWidth) - (2.0f * ofsX)) / (slotsPerRow - 1));
    float vertSeparator = ((worldHeight - (slotsPerColumn * cardHeight) - (2.0f * ofsY)) / (slotsPerColumn - 1));
    int cardIdx = 0;

    for (int y = 0; y < slotsPerColumn; ++y) {
        for (int x = 0; x < slotsPerRow; ++x) {
            float posX = ofsX + (x * (cardWidth + horizSeparator)) - (worldWidth * 0.5f) + (cardWidth * 0.5f);
            float posY = ofsY + (y * (cardHeight + vertSeparator)) - (worldHeight * 0.5f) + (cardHeight * 0.5f);
            this.Cards[cardsIndexes[cardIdx]].transform.position = new Vector3(posX, posY);
            cardIdx++;
        }
    }
}

private void OnResize(int newScreenWidth, int newScreenHeight)
{
    // resize the background
    this.ResizeBackground(newScreenWidth, newScreenHeight);
    // resize animals sprites
    this.ResizeCards(newScreenWidth, newScreenHeight);
    // rearrange cards on the screen
    this.DisposeCards();
}

private static readonly int[] CARDS_INDEXES_PORTRAIT = { 0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11 };
private static readonly int[] CARDS_INDEXES_LANDSCAPE = { 9, 6, 3, 0, 10, 7, 4, 1, 11, 8, 5, 2 };

The code is really simple, here are some notes:

- when sprites are regenerated at runtime, due to a resolution change, the relative box colliders are update too ('UpdateCardsSprites' function)
- if the screen has a landscape layout, the cards are arranged on 3 rows and 4 columns; on portrait layouts, instead, they are arranged on 4 rows and 3 columns ('DisposeCards' function)

The 'StartNewGame' function has been modified in order to generate six pairs of animals (12 cards in total), randomly chosen among the 25 available:

public void ShowCard(GameExampleCardBehaviour card)
{
    card.Active = true;
    // enable renderer
    card.gameObject.GetComponent<SpriteRenderer>().enabled = true;
    // enable collider
    card.GetComponent<BoxCollider2D>().enabled = true;
}

private void Shuffle(GameExampleCardType[] array)
{
    System.Random rnd = new System.Random(System.Environment.TickCount);
    int n = array.Length;
    // Knuth shuffle
    while (n > 1)
    {
        n--;
        int i = rnd.Next(n + 1);
        GameExampleCardType temp = array[i];
        array[i] = array[n];
        array[n] = temp;
    }
}

public void StartNewGame()
{
    GameExampleCardType[] animalCouples = new GameExampleCardType[this.Cards.Length];
    // start with a random animal
    GameExampleCardType currentAnimal = GameExampleCardBehaviour.RandomAnimal();

    // generate animal couples
    for (int i = 0; i < (this.Cards.Length / 2); ++i)
    {
        animalCouples[i * 2] = currentAnimal;
        animalCouples[(i * 2) + 1] = currentAnimal;
        currentAnimal = GameExampleCardBehaviour.NextAnimal(currentAnimal);
    }

    // shuffle couples
    this.Shuffle(animalCouples);

    // assign cards
    for (int i = 0; i < this.Cards.Length; ++i)
    {
        this.Cards[i].AnimalType = animalCouples[i];
        this.ShowCard(this.Cards[i]);
    }

    // destroy current background texture
    this.Background.DestroyAll(true);
    // assign a new SVG file
    this.Background.SVGFile = this.BackgroundFiles[this.m_BackgroundIndex % this.BackgroundFiles.Length];
    // advance for the next background SVG
    this.m_BackgroundIndex++;
}

[ unity_tut3_cards_shuffled.png ](All the graphics are perfectly sized and respect the screen layout)

The last detail that is missing is how we detect if a card is selected by mouse/touch actions. This is really simple, because we have already configured box colliders on cards: we just need to write the 'OnMouseDown' event handler within the 'GameExampleCardBehaviour' script.

void OnMouseDown()
{
    // forward the selection event to the Game
    this.Game.SelectCard(this);
}

The remaining code (not reported here because really trivial) simply deals with the gameplay: all the cards must start as backside and if the two selected cards match, they are made inactive ('GameExampleCardBehaviour.Active = false') and removed from the game, otherwise they are covered again. The game ends when all the cards are inactive (i.e. all pairs of animals have been discovered).

[ unity_tut3_the_game.png ](A new game and we immediately chose a pair that does not match!)

The complete project can be found [here](http://github.com/Mazatech/amanithsvg-bindings/tree/master/Unity) (the [game scene](http://github.com/Mazatech/amanithsvg-bindings/blob/master/Unity/Assets/SVGAssets/Scenes/game.unity)).
As it can be seen, card objects have also an [Animation](http://docs.unity3d.com/Manual/class-Animation.html) component attached, used to perform a simple rotation when shuffling or when two selected cards match.

Now you can have fun experimenting with it!eAtlasGenerator(
    	                            1.0f,
    	                            Math.min(SVGTextureUtils.getGlMaxTextureDimension(),
    	                                     AmanithSVG.svgtSurfaceMaxDimension()),
    	                            1, false, false, SVGColor.Clear);

    // SVG file, explodeGroups, scale
    atlasGen.add(Gdx.files.internal("animals.svg"), true, 0.65f);
    // NB: because 'animals.svg' has been designed for a 768 x 640 resolution (see the file
    // header), we do not want to adjust the scale further (i.e. we pass 1.0f as additional
    // scale factor)
    // Note that at the 768 x 640 'reference resolution', each animal sprite will have a 
    // dimension of 128 x 128: it will ALWAYS guarantee that we can easily place them on a
    // 4 x 3 grid (landscape layout) or on a 3 x 4 grid (portrait layout), REGARDLESS OF 
    // SCREEN RESOLUTION

    // the map will associate each animal type to the respective texture region
    animalsSprites = new HashMap<CardType, SVGTextureAtlasRegion>();
}
```

Now we want to regenerate animal sprites at each `resize` event, as we already did for the background:

```java

private SVGTextureAtlas atlas = null;

private void generateAnimalSprites(int screenWidth, int screenHeight) {

    // calculate the scale factor according to the current window/screen resolution
    float scale = scaler.scaleFactorCalc(screenWidth, screenHeight);

    // set generation scale (all other parameters have been set when instantiating
    // the SVGTextureAtlasGenerator class)
    atlasGen.setScale(scale);

    // dispose previous textures atlas
    if (atlas != null) {
        atlas.dispose();
    }

    // do the real generation
    atlas = atlasGen.generateAtlas();

    // empty the previous map
    animalsSprites.clear();

    // now associate to each animal type (i.e. the key) the respective
    // texture region (i.e. the value)
    for (SVGTextureAtlasPage page : atlas.getPages()) {
        for (SVGTextureAtlasRegion region : page.getRegions()) {
            animalsSprites.put(CardType.fromName(region.getElemName()), region);
        }
    }
}

@Override public void resize(int width, int height) {

    ...

    if ((width > 0) && (height > 0)) {
        // generate background
        generateBackground(width, height);
        // generate sprites
        generateAnimalSprites(width, height);
    }
}
```

Now the easiest part! We only have to instantiate six pairs of animals (12 cards in total), randomly chosen among the 25 available:

```java
// the deck of cards
private Card[] cards = null;

private void startNewGame() {

    CardType[] animalCouples = new CardType[12];
    // start with a random animal
    CardType currentAnimal = CardType.random();

    // generate animal couples
    for (int i = 0; i < 6; ++i) {
        animalCouples[i * 2] = currentAnimal;
        animalCouples[(i * 2) + 1] = currentAnimal;
        currentAnimal = currentAnimal.next();
    }

    // shuffle couples (Knuth shuffle)
    Random rnd = new Random();
    int n = animalCouples.length;
    while (n > 1) {
        n--;
        int i = rnd.nextInt(n + 1);
        CardType temp = animalCouples[i];
        animalCouples[i] = animalCouples[n];
        animalCouples[n] = temp;
    }

    // assign cards
    for (int i = 0; i < 12; ++i) {
        // cards start as active and backside
        cards[i].active = true;
        cards[i].backSide = true;
        cards[i].animalType = animalCouples[i];
    }
}

@Override public void create() {

    ...

    // create cards array
    cards = new Card[12];
    for (int i = 0; i < 12; ++i) {
        cards[i] = new Card();
    }

    // start a new game (i.e. select random cards and initilize them as "backside")
    startNewGame();
}
```

The rendering of cards is really simple, it's just a matter of looping over them and:

 - if the card is backside (`Card.backSide == true`), select the sprite associated to the `CardType.BackSide` enum value, else

 - if the card is not backside (`Card.backSide == false`), select the sprite associated to the `Card.animalType` field

 - draw the selected sprite (actually a [SVGTextureAtlasRegion]({{site.url}}/docs/binding/003-libgdx.html#svgtextureatlasregion)) using the libGDX `SpriteBatch` [draw](http://libgdx.badlogicgames.com/ci/nightlies/docs/api/com/badlogic/gdx/graphics/g2d/SpriteBatch.html#draw-com.badlogic.gdx.graphics.g2d.TextureRegion-float-float-) method.

```java
@Override public void render() {

    ...

    batch.begin();
    // draw the background (texture, x, y,
    //                      width, height,
    //                      srcX, srcY, srcWidth, srcHeight,
    //                      flipX, flipY)
    batch.draw(backgroundTexture, 0.0f, 0.0f,
               Gdx.graphics.getBackBufferWidth(), Gdx.graphics.getBackBufferHeight(),
               0, 0, backgroundTexture.getWidth(), backgroundTexture.getHeight(),
               false, true);
    // draw cards
    for (int i = 0; i < 12; ++i) {
        // draw active cards only
        if (cards[i].active) {
            SVGTextureAtlasRegion region = animalsSprites.get(cards[i].backSide ?
                                                              CardType.BackSide :
                                                              cards[i].animalType);
            batch.draw(region, cards[i].x, cards[i].y);
        }
    }
    batch.end();
}
```

The last detail that is missing is how the 12 cards are disposed on the screen. If the screen has a landscape layout, the cards are arranged on 3 rows and 4 columns; on portrait layouts, instead, they are arranged on 4 rows and 3 columns.

```java
private static int[] CARDS_INDEXES_PORTRAIT = { 0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11 };
private static int[] CARDS_INDEXES_LANDSCAPE = { 9, 6, 3, 0, 10, 7, 4, 1, 11, 8, 5, 2 };

private void disposeCards(int screenWidth, int screenHeight) {

    int[] cardsIndexes;
    int slotsPerRow, slotsPerColumn;
    SVGTextureAtlasRegion region = animalsSprites.get(CardType.BackSide);
    int cardWidth = region.getRegionWidth();
    int cardHeight = region.getRegionWidth();

    if (screenWidth <= screenHeight) {
        // number of card slots in each dimension
        slotsPerRow = 3;
        slotsPerColumn = 4;
        cardsIndexes = MyGdxGame.CARDS_INDEXES_PORTRAIT;
    }
    else {
        // number of card slots in each dimension
        slotsPerRow = 4;
        slotsPerColumn = 3;
        cardsIndexes = MyGdxGame.CARDS_INDEXES_LANDSCAPE;
    }

    // 5% border
    int ofsX = (int)Math.floor(screenWidth * 0.05);
    int ofsY = (int)Math.floor(screenHeight * 0.05);
    int horizSeparator = ((screenWidth - (slotsPerRow * cardWidth) - (2 * ofsX)) / (slotsPerRow - 1));
    int vertSeparator = ((screenHeight - (slotsPerColumn * cardHeight) - (2 * ofsY)) / (slotsPerColumn - 1));
    int cardIdx = 0;

    for (int y = 0; y < slotsPerColumn; ++y) {
        for (int x = 0; x < slotsPerRow; ++x) {
            region = animalsSprites.get(cards[cardsIndexes[cardIdx]].animalType);
            cards[cardsIndexes[cardIdx]].x = ofsX + (x * (cardWidth + horizSeparator));
            cards[cardsIndexes[cardIdx]].y = ofsY + (y * (cardHeight + vertSeparator));
            cards[cardsIndexes[cardIdx]].width = region.getRegionWidth();
            cards[cardsIndexes[cardIdx]].height = region.getRegionHeight();
            cardIdx++;
        }
    }
}

@Override public void resize(int width, int height) {

    ...

    if ((width > 0) && (height > 0)) {
        // generate background
        generateBackground(width, height);
        // generate sprites
        generateAnimalSprites(width, height);
        // place cards on the screen
        disposeCards(width, height);
    }
}
```

| ![Landscape layout: 3 rows and 4 columns]({{site.url}}/assets/images/libgdx_tut1_landscape_layout.png) |
| :---: |
| *Landscape layout: 3 rows and 4 columns* |

| ![Portrait layout: 4 rows and 3 columns]({{site.url}}/assets/images/libgdx_tut1_portrait_layout.png) |
| :---: |
| *Portrait layout: 4 rows and 3 columns* |

The game class will also implement [InputProcessor](http://libgdx.badlogicgames.com/ci/nightlies/docs/api/com/badlogic/gdx/InputProcessor.html), so that it can intercept mouse and touch events.
Please refer to the [official guide](http://github.com/libgdx/libgdx/wiki/Mouse%2C-Touch-and-Keyboard) for code details. Once that a mouse/touch event has been caught, it is passed to the following function that simply checks if a card has been "selected" (a trivial "point in a box" test):

```java
void selectCard(int touchX, int touchY) {

    for (Card card : cards) {
        if (card.active) {
            // check if the card has been touched
            if ((touchX > card.x) && (touchX < (card.x + card.width)) &&
                (touchY > card.y) && (touchY < (card.y + card.height))) {
                ...
                break;
            }
        }
    }
}
```

The remaining code (not reported here because really trivial) simply deals with the gameplay: if the two selected cards match, they are made inactive (`Card.active = false`) and removed from the game, otherwise they are covered again. The game ends when all the cards are inactive (i.e. all pairs of animals have been discovered).

| ![A new game and we immediately chose a pair that does not match!]({{site.url}}/assets/images/libgdx_tut1_the_game.png) |
| :---: |
| *A new game and we immediately chose a pair that does not match!* |

The complete project can be found [here](http://github.com/Mazatech/amanithsvg-bindings/tree/master/libGDX/gameCards). Now you can have fun experimenting with it!

---