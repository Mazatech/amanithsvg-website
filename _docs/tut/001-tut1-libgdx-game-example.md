---
layout: default
title: "libGDX - game example"
date: 2018-01-01 08:00:00 +0100
chapter: 1
categories: [tut]
---

# libGDX memory game example

In this tutorial, we will create a simple prototype of a memory-like game, using [libGDX](https://libgdx.com) and the relative [AmanithSVG binding API]({{site.url}}/docs/binding/003-libgdx.html). We will create the project following the [official guide](https://libgdx.com/wiki/start/project-generation).

The complete project can be found [here](http://github.com/Mazatech/amanithsvg-sdk/tree/master/examples/libgdx/gameCards)

---

## The setup

First, lets create the project structure:

 - Make sure all the required libGDX [prerequirements](https://libgdx.com/wiki/start/setup) are met. Especially those concerning the [command line](https://libgdx.com/wiki/start/setup#5-no-ide), because we will use this approach to build the whole project 

 - Download libGDX [Project Setup Tool](https://libgdx-nightlies.s3.amazonaws.com/libgdx-runnables/gdx-setup.jar)

 - Open up your command line tool, go to the download folder and run:

    ```
    java -jar gdx-setup.jar
    ```

 - Fill all required fields as show in the following image

    | &nbsp; |
    | :---: |
    | *Project setup using the command line* |
    {:.tbl_images .libgdx_tut1_setup_1}

Now the project folders structure should look like the following:

| &nbsp; |
| :---: |
| *The project folders structure* |
{:.tbl_images .libgdx_tut1_setup_2}

The next step is to include the [AmanithSVG binding for libGDX]({{site.url}}/docs/binding/003-libgdx.html) to the project:

 - copy the [com.mazatech.svgt](https://github.com/Mazatech/amanithsvg-sdk/tree/master/examples/libgdx/gameCards/core/src/com/mazatech/svgt) sources within the project's `core/src` folder

 - copy the [com.mazatech.gdx](https://github.com/Mazatech/amanithsvg-sdk/tree/master/examples/libgdx/gameCards/core/src/com/mazatech/gdx) sources within the project's `core/src` folder

 - edit the `build.gradle` file present within the project main directory:

    - add `amanithsvgVersion = '2.0.1'` to the `allprojects` → `ext` section

    - add `implementation "com.mazatech.amanithsvg:amanithsvg-gdx:$amanithsvgVersion:natives-desktop"` to the `project(":desktop")` → `dependencies` section

    - add `natives "com.mazatech.amanithsvg:amanithsvg-gdx:$amanithsvgVersion:natives-armeabi"` to the `project(":android")` → `dependencies` section

    - add `natives "com.mazatech.amanithsvg:amanithsvg-gdx:$amanithsvgVersion:natives-armeabi-v7a"` to the `project(":android")` → `dependencies` section

    - add `natives "com.mazatech.amanithsvg:amanithsvg-gdx:$amanithsvgVersion:natives-arm64-v8a"` to the `project(":android")` → `dependencies` section

    - add `natives "com.mazatech.amanithsvg:amanithsvg-gdx:$amanithsvgVersion:natives-x86"` to the `project(":android")` → `dependencies` section

    - add `natives "com.mazatech.amanithsvg:amanithsvg-gdx:$amanithsvgVersion:natives-x86_64"` to the `project(":android")` → `dependencies` section

    - add `implementation "com.mazatech.amanithsvg:amanithsvg-gdx:$amanithsvgVersion:natives-ios"` to the `project(":ios")` → `dependencies` section

    | &nbsp; |
    | :---: |
    | *AmanithSVG native library dependencies added to build.gradle file* |
    {:.tbl_images .libgdx_tut1_setup_3}

After these operations, the `build.gradle` file should look like [this](https://github.com/Mazatech/amanithsvg-sdk/blob/master/examples/libgdx/gameCards/build.gradle) and the folders structure should look like the following:

| &nbsp; |
| :---: |
| *The project now includes AmanithSVG packages* |
{:.tbl_images .libgdx_tut1_setup_4}

Now we are ready to check the project setup by running, from the project main directory, the command `./gradlew desktop:run` on Linux and MacOS X systems or `gradlew desktop:run` on Windows systems.
If all is ok, you should be able to see the classic libGDX "Hello world"-like window.

| &nbsp; |
| :---: |
| *Project setup has been completed successfully* |
{:.tbl_images .libgdx_tut1_setup_5}

---

## The basic template

From now until the end we will work on the `MyGdxGame.java` file. First of all we remove the Badlogic Games image logo, and we plug AmanithSVG in. To do so:

 - we import `com.mazatech.svgt` and `com.mazatech.gdx` packages

 - we initialize AmanithSVG library within the [create](https://javadoc.io/static/com.badlogicgames.gdx/gdx/1.11.0/com/badlogic/gdx/ApplicationAdapter.html#create--) function, by instantiating the `SVGAssetsGDX` class

 - we release AmanithSVG within the [dispose](https://javadoc.io/static/com.badlogicgames.gdx/gdx/1.11.0/com/badlogic/gdx/ApplicationAdapter.html#dispose--) function, by calling `SVGAssetsGDX.dispose()`

The complete basic template of our game will look as follow:

```java
package com.mygdx.game;

// libGDX
import com.badlogic.gdx.Application;
import com.badlogic.gdx.ApplicationAdapter;
import com.badlogic.gdx.Gdx;
import com.badlogic.gdx.graphics.g2d.SpriteBatch;
import com.badlogic.gdx.graphics.glutils.HdpiUtils;
import com.badlogic.gdx.graphics.OrthographicCamera;
import com.badlogic.gdx.utils.ScreenUtils;

// AmanithSVG for libGDX
import com.mazatech.gdx.SVGAssetsGDX;
import com.mazatech.gdx.SVGAssetsConfigGDX;

// AmanithSVG java binding (high level layer)
import com.mazatech.svgt.SVGAssets;

public class MyGdxGame extends ApplicationAdapter {

    private static final String LOG_TAG = "MyGdxGame";

    private SpriteBatch batch;
    private OrthographicCamera camera;
    // instance of AmanithSVG for libGDX
    private SVGAssetsGDX svg = null;

    // display some basic information about installed AmanithSVG library
    void amanithsvgInfoDisplay() {

        // vendor
        Gdx.app.log(LOG_TAG, "AmanithSVG vendor = " + SVGAssets.getVendor());
        // version
        Gdx.app.log(LOG_TAG, "AmanithSVG version = " + SVGAssets.getVersion());
    }

    @Override
    public void create() {

        // get actual backbuffer resolution
        int screenWidth = Gdx.graphics.getBackBufferWidth();
        int screenHeight = Gdx.graphics.getBackBufferHeight();
        // create configuration for AmanithSVG
        SVGAssetsConfigGDX cfg = new SVGAssetsConfigGDX(screenWidth,
                                                        screenHeight,
                                                        Gdx.graphics.getPpiX());
        // initialize AmanithSVG for libGDX
        svg = new SVGAssetsGDX(cfg);
        // create the batch (used by 'render' function)
        batch = new SpriteBatch();
        // setup orthographic camera
        camera = new OrthographicCamera();

        // display some basic information about installed AmanithSVG library
        amanithsvgInfoDisplay();
    }

    @Override
    public void resize(int width,
                       int height) {

        if ((width > 0) && (height > 0)) {
            // update OpenGL viewport
            HdpiUtils.glViewport(0, 0, width, height);
        }
    }

    @Override
    public void render() {

        // get actual backbuffer resolution
        int screenWidth = Gdx.graphics.getBackBufferWidth();
        int screenHeight = Gdx.graphics.getBackBufferHeight();

        // clear screen
        ScreenUtils.clear(1, 1, 1, 1);

        // setup orthographic camera
        camera.setToOrtho(false, screenWidth, screenHeight);
        camera.update();
        batch.setProjectionMatrix(camera.combined);
    }

    @Override
    public void dispose() {

        // release libGDX resources
        batch.dispose();
        // release AmanithSVG
        svg.dispose();
    }
}
```

If you run the project, you will get the following result:

| &nbsp; |
| :---: |
| *The basic template of our game* |
{:.tbl_images .libgdx_tut1_basic_template}

---

## The game background

Now it's time to add some cute SVG backgrounds to our game, so lets start to copy [gameBkg1.svg](https://github.com/Mazatech/amanithsvg-sdk/blob/master/examples/libgdx/gameCards/android/assets/gameBkg1.svg), [gameBkg2.svg](https://github.com/Mazatech/amanithsvg-sdk/blob/master/examples/libgdx/gameCards/android/assets/gameBkg2.svg), [gameBkg3.svg](https://github.com/Mazatech/amanithsvg-sdk/blob/master/examples/libgdx/gameCards/android/assets/gameBkg3.svg), [gameBkg4.svg](https://github.com/Mazatech/amanithsvg-sdk/blob/master/examples/libgdx/gameCards/android/assets/gameBkg4.svg) files to the project's 'android/assets' folder.

We store the [SVGDocument]({{site.url}}/docs/binding/002-java.html#svgdocument) instances relative to the four backgrounds and keep track of the generated background [texture]({{site.url}}/docs/binding/003-libgdx.html#svgtexture):

```java
// SVG background documents
private SVGDocument[] backgroundDocs = { null, null, null, null };
// the actual background texture
private SVGTexture backgroundTexture = null;
```

We load all four SVG backgrounds files at initialization time (`create` function). Please note that we override the default [alignment]({{site.url}}/docs/binding/002-java.html#svgalignment), because we want the background to cover the whole screen.

```java
@Override
public void create() {

    // get actual backbuffer resolution
    int screenWidth = Gdx.graphics.getBackBufferWidth();
    int screenHeight = Gdx.graphics.getBackBufferHeight();
    // create configuration for AmanithSVG; NB: use the backbuffer to get the real dimensions in pixels
    SVGAssetsConfigGDX cfg = new SVGAssetsConfigGDX(screenWidth,
                                                    screenHeight,
                                                    Gdx.graphics.getPpiX());
    // initialize AmanithSVG for libGDX
    svg = new SVGAssetsGDX(cfg);

    ...

    // load backgrounds SVG documents
    for (int i = 0; i < 4; ++i) {
        backgroundDocs[i] = svg.createDocumentFromFile("gameBkg" + (i + 1) + ".svg");
        // backgrounds viewBox must cover the whole drawing surface, so we use
        // SVGTMeetOrSlice.Slice (default is SVGTMeetOrSlice.Meet)
        backgroundDocs[i].setAspectRatio(SVGTAlign.XMidYMid, SVGTMeetOrSlice.Slice);
    }

    ...
}
```

We resize the background (i.e. we generate the background texture at a new resolution) at each `resize` event:

```java
void generateBackground(int screenWidth,
                        int screenHeight) {

    // destroy previous backgound texture
    if (backgroundTexture != null) {
        backgroundTexture.dispose();
    }

    // generate a new background texture
    backgroundTexture = svg.createTexture(backgroundDocs[backgroundIdx],
                                          screenWidth,
                                          screenHeight);
}

@Override
public void resize(int width,
                   int height) {

    if ((width > 0) && (height > 0)) {
        // get actual backbuffer resolution
        int screenWidth = Gdx.graphics.getBackBufferWidth();
        int screenHeight = Gdx.graphics.getBackBufferHeight();
        // update OpenGL viewport
        HdpiUtils.glViewport(0, 0, width, height);
        // generate background
        generateBackground(screenWidth, screenHeight);
    }
}
```

We draw the background texture within the `render` function:

```java
// draw the game, just the background for the moment
private void drawGame(int screenWidth,
                      int screenHeight) {

    // clear screen
    ScreenUtils.clear(1, 1, 1, 1);

    // start drawing
    batch.begin();
    // draw the background
    batch.draw(backgroundTexture,
               // destination screen region
               0, 0, screenWidth, screenHeight,
               // source texture region
               0, 0, backgroundTexture.getWidth(), backgroundTexture.getHeight(),
               // flipX, flipY
               false, true);
    // finish drawing
    batch.end();
}

@Override
public void render() {

    // get actual backbuffer resolution
    int screenWidth = Gdx.graphics.getBackBufferWidth();
    int screenHeight = Gdx.graphics.getBackBufferHeight();

    // setup orthographic camera
    camera.setToOrtho(false, screenWidth, screenHeight);
    camera.update();
    batch.setProjectionMatrix(camera.combined);

    // draw the game
    drawGame(screenWidth, screenHeight);
}
```

And finally we dispose the four background documents and the background texture when the application is being closed (`dispose` function):

```java
@Override
public void dispose() {

    // release AmanithSVG resources
    for (int i = 0; i < 4; ++i) {
        backgroundDocs[i].dispose();
    }
    // release libGDX resources
    backgroundTexture.dispose();
    batch.dispose();

    // release AmanithSVG
    svg.dispose();
}
```

| &nbsp; |
| :---: |
| *Backgrounds used within the game* |
{:.tbl_images .libgdx_tut1_four_backgrounds}

---

## The cards

The characters hidden behind the cards are cute animals, here are their name (see full implementation [here](https://github.com/Mazatech/amanithsvg-sdk/blob/master/examples/libgdx/gameCards/core/src/com/mazatech/amanithsvg/gamecards/CardType.java)):

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

We model a single card as a set of basic attributes (see full implementation [here](https://github.com/Mazatech/amanithsvg-sdk/blob/master/examples/libgdx/gameCards/core/src/com/mazatech/amanithsvg/gamecards/Card.java)):

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
<!DOCTYPE svg PUBLIC "-//W3C//DTD SVG 1.1//EN" "https://www.w3.org/Graphics/SVG/1.1/DTD/svg11.dtd">
<svg version="1.1"
	 id="Animals"
	 xmlns="https://www.w3.org/2000/svg"
	 xmlns:xlink="https://www.w3.org/1999/xlink"
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

| &nbsp; |
| :---: |
| *gameAnimals.svg* |
{:.tbl_images .animalsSvg}

Lets start by copying [gameAnimals.svg](https://github.com/Mazatech/amanithsvg-sdk/blob/master/examples/libgdx/gameCards/android/assets/gameAnimals.svg) to the project's `android/assets` folder.
Now we want to implement a function that generates animals sprites from the given SVG file, taking care of the current screen resolution. This is easy with AmanithSVG, we make use of [SVGTextureAtlasGenerator]({{site.url}}/docs/binding/003-libgdx.html#svgtextureatlasgenerator) class and the [SVGScaler](https://github.com/Mazatech/amanithsvg-sdk/blob/master/examples/libgdx/gameCards/core/src/com/mazatech/svgt/SVGScaler.java) utility. Along with the animal sprites creation, we want to create a 1-1 [map](https://docs.oracle.com/javase/7/docs/api/java/util/Map.html) between each sprite and the relative `CardType` enum value.

First of all we load `gameAnimals.svg` file and create an instance of `SVGTextureAtlasGenerator` within the `create` function:

```java
// SVG atlas generator
private SVGTextureAtlasGenerator atlasGen = null;
// associate each animal type the respective texture region
private HashMap<CardType, SVGTextureAtlasRegion> animalsSprites = null;

@Override
public void create() {

    // initialize AmanithSVG for libGDX
    ...

    // create backgrounds documents
    ...

    // scale, border, dilateEdgesFix
    atlasGen = svg.createAtlasGenerator(1, 1, false);
    // SVG file, explodeGroups, scale
    atlasGen.add("gameAnimals.svg", true, 1);
```

Now we want to regenerate animal sprites at each `resize` event, as we already did for the background:

```java
private SVGTextureAtlas atlas = null;

private void generateAnimalSprites(int screenWidth,
                                   int screenHeight) {

    // the scaler will calculate the correct scaling factor, actual parameters say:
    // "We have created all the SVG files (that we are going to pack in atlas) so
    // that, at 768 x 640 (the 'reference resolution'), they do not need additional
    // scaling (the last passed parameter value 1 is the basic scale relative to
    // the 'reference resolution'); if the device has a different screen resolution,
    // we want to scale SVG contents depending on the actual width and height
    // (MatchWidthOrHeight), equally important (0.5f)"
    SVGScaler scaler = new SVGScaler(768, 640, SVGScalerMatchMode.MatchWidthOrHeight, 0.5f, 1);
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

@Override
public void resize(int width,
                   int height) {

    if ((width > 0) && (height > 0)) {
        // get actual backbuffer resolution
        int screenWidth = Gdx.graphics.getBackBufferWidth();
        int screenHeight = Gdx.graphics.getBackBufferHeight();
        // update OpenGL viewport
        HdpiUtils.glViewport(0, 0, width, height);
        // generate background
        generateBackground(screenWidth, screenHeight);
        // generate sprites
        generateAnimalSprites(screenWidth, screenHeight);
    }
}
```

Now the easiest part! We only have to instantiate six pairs of animals (12 cards in total), randomly chosen among the 25 available:

```java
// number of cards
private static final int CARDS_COUNT = 12;
// the deck of cards
private Card[] cards = new Card[CARDS_COUNT];

private void startNewGame() {

    CardType[] animalCouples = new CardType[CARDS_COUNT];
    // start with a random animal
    CardType currentAnimal = CardType.random();

    // generate animal couples
    for (int i = 0; i < (CARDS_COUNT / 2); ++i) {
        animalCouples[i * 2] = currentAnimal;
        animalCouples[(i * 2) + 1] = currentAnimal;
        currentAnimal = currentAnimal.next();
    }

    // shuffle couples (Knuth shuffle)
    Random rnd = new Random();
    int n = CARDS_COUNT;
    while (n > 1) {
        n--;
        int i = rnd.nextInt(n + 1);
        CardType temp = animalCouples[i];
        animalCouples[i] = animalCouples[n];
        animalCouples[n] = temp;
    }

    // assign cards
    for (int i = 0; i < CARDS_COUNT; ++i) {
        // cards start as active and backside
        cards[i].active = true;
        cards[i].backSide = true;
        cards[i].animalType = animalCouples[i];
    }
}

@Override
public void create() {

    ...

    // start a new game (i.e. select random cards and initilize them as "backside")
    startNewGame();
}
```

The rendering of cards is really simple, it's just a matter of looping over them and:

 - if the card is backside (`Card.backSide == true`), select the sprite associated to the `CardType.BackSide` enum value, else

 - if the card is not backside (`Card.backSide == false`), select the sprite associated to the `Card.animalType` field

 - draw the selected sprite (actually a [SVGTextureAtlasRegion]({{site.url}}/docs/binding/003-libgdx.html#svgtextureatlasregion)) using the libGDX `SpriteBatch` [draw](https://javadoc.io/static/com.badlogicgames.gdx/gdx/1.11.0/com/badlogic/gdx/graphics/g2d/SpriteBatch.html#draw-com.badlogic.gdx.graphics.Texture-float-float-float-float-int-int-int-int-boolean-boolean-) method.

```java
// draw the game, background and cards
private void drawGame(int screenWidth,
                      int screenHeight) {

    // clear screen
    ScreenUtils.clear(1, 1, 1, 1);

    // start drawing
    batch.begin();
    // draw the background
    batch.draw(backgroundTexture,
               // destination screen region
               0, 0, screenWidth, screenHeight,
               // source texture region
               0, 0, backgroundTexture.getWidth(), backgroundTexture.getHeight(),
               // flipX, flipY
               false, true);
    // draw cards
    for (Card card : cards) {
        // draw active cards only
        if (card.active) {
            SVGTextureAtlasRegion region = animalsSprites.get(card.backSide ?
                                                              CardType.BackSide :
                                                              card.animalType);
            batch.draw(region, card.x, card.y);
        }
    }
    // finish drawing
    batch.end();
}
```

The last detail that is missing is how the 12 cards are disposed on the screen. If the screen has a landscape layout, the cards are arranged on 3 rows and 4 columns; on portrait layouts, instead, they are arranged on 4 rows and 3 columns.

```java
// cards arrangement when native device orientation is portrait
private static final int[] CARDS_INDEXES_NATIVE_PORTRAIT = {
    0,  1,  2,
    3,  4,  5,
    6,  7,  8,
    9, 10, 11
};
// landscape orientation, clockwise from the portrait orientation
private static final int[] CARDS_INDEXES_LANDSCAPE_ROT90 = {
    9, 6, 3, 0,
    10, 7, 4, 1,
    11, 8, 5, 2
};
// landscape orientation, counter-clockwise from the portrait orientation
private static final int[] CARDS_INDEXES_LANDSCAPE_ROT270 = {
    2, 5, 8, 11,
    1, 4, 7, 10,
    0, 3, 6,  9
};

// cards arrangement when native device orientation is landscape
private static final int[] CARDS_INDEXES_NATIVE_LANDSCAPE = {
    0,  1,  2, 3,
    4,  5,  6, 7,
    8,  9, 10, 11
};
private static final int[] CARDS_INDEXES_PORTRAIT_ROT90 = {
    8, 4, 0,
    9, 5, 1,
    10, 6, 2,
    11, 7, 3
};
private static final int[] CARDS_INDEXES_PORTRAIT_ROT270 = {
    3, 7, 11,
    2, 6, 10,
    1, 5, 9,
    0, 4, 8
};

private void placeCards(int screenWidth,
                        int screenHeight) {

    int[] cardsIndexes;
    Application.ApplicationType appType = Gdx.app.getType();
    SVGTextureAtlasRegion region = animalsSprites.get(CardType.BackSide);
    int cardWidth = region.getRegionWidth();
    int cardHeight = region.getRegionWidth();
    // number of card slots in each dimension
    int slotsPerRow = (screenWidth <= screenHeight) ? 3 : 4;
    int slotsPerColumn = (screenWidth <= screenHeight) ? 4 : 3;
    // 5% border
    int borderX = (int)Math.floor(screenWidth * 0.05);
    int borderY = (int)Math.floor(screenHeight * 0.05);
    // space between one card and the adjacent one
    int horizSeparator = ((screenWidth - (slotsPerRow * cardWidth) - (2 * borderX)) / (slotsPerRow - 1));
    int vertSeparator = ((screenHeight - (slotsPerColumn * cardHeight) - (2 * borderY)) / (slotsPerColumn - 1));
    int i = 0;

    // check actual orientation on iOS and Android devices
    if ((appType == Application.ApplicationType.Android) ||
        (appType == Application.ApplicationType.iOS)) {

        // get current orientation
        int deviceRotation = Gdx.input.getRotation();

        if (nativeDeviceOrientation == Input.Orientation.Portrait) {
            // native orientation is portrait, now handle rotations
            switch (deviceRotation) {
                case 90:
                    cardsIndexes = CARDS_INDEXES_LANDSCAPE_ROT90;
                    break;
                case 270:
                    cardsIndexes = CARDS_INDEXES_LANDSCAPE_ROT270;
                    break;
                default:
                    cardsIndexes = CARDS_INDEXES_NATIVE_PORTRAIT;
                    break;
            }
        }
        else {
            // native orientation is landscape, now handle rotations
            switch (deviceRotation) {
                case 90:
                    cardsIndexes = CARDS_INDEXES_PORTRAIT_ROT90;
                    break;
                case 270:
                    cardsIndexes = CARDS_INDEXES_PORTRAIT_ROT270;
                    break;
                default:
                    cardsIndexes = CARDS_INDEXES_NATIVE_LANDSCAPE;
                    break;
            }
        }
    }
    else {
        // Desktop, HeadlessDesktop, Applet; detect orientation according to screen dimensions
        cardsIndexes = (screenWidth <= screenHeight) ? CARDS_INDEXES_NATIVE_PORTRAIT
                                                     : CARDS_INDEXES_NATIVE_LANDSCAPE;
    }

    for (int y = 0; y < slotsPerColumn; ++y) {
        for (int x = 0; x < slotsPerRow; ++x) {
            region = animalsSprites.get(cards[cardsIndexes[i]].animalType);
            cards[cardsIndexes[i]].x = borderX + (x * (cardWidth + horizSeparator));
            cards[cardsIndexes[i]].y = borderY + (y * (cardHeight + vertSeparator));
            cards[cardsIndexes[i]].width = region.getRegionWidth();
            cards[cardsIndexes[i]].height = region.getRegionHeight();
            i++;
        }
    }
}

@Override
public void resize(int width,
                   int height) {

    if ((width > 0) && (height > 0)) {
        // get actual backbuffer resolution
        int screenWidth = Gdx.graphics.getBackBufferWidth();
        int screenHeight = Gdx.graphics.getBackBufferHeight();
        // update OpenGL viewport
        HdpiUtils.glViewport(0, 0, width, height);
        // generate background
        generateBackground(screenWidth, screenHeight);
        // generate sprites
        generateAnimalSprites(screenWidth, screenHeight);
        // place cards on the screen
        placeCards(screenWidth, screenHeight);
    }
}
```

| &nbsp; |
| :---: |
| *Landscape layout: 3 rows and 4 columns* |
{:.tbl_images .libgdx_tut1_landscape_layout}

| &nbsp; |
| :---: |
| *Portrait layout: 4 rows and 3 columns* |
{:.tbl_images .libgdx_tut1_portrait_layout}

The game class will also implement [InputProcessor](https://javadoc.io/static/com.badlogicgames.gdx/gdx/1.11.0/com/badlogic/gdx/InputProcessor.html), so that it can intercept mouse and touch events.
Please refer to the [official guide](https://libgdx.com/wiki/input/mouse-touch-and-keyboard) for code details. Once that a mouse/touch event has been caught, it is passed to the following function that simply checks if a card has been "selected" (a trivial "point in a box" test):

```java
private void selectCard(int screenX,
                        int screenY) {

    for (Card card : cards) {
        if (card.active) {
            // check if the card has been touched
            if ((screenX > card.x) && (screenX < (card.x + card.width)) &&
                (screenY > card.y) && (screenY < (card.y + card.height))) {
                ...
                break;
            }
        }
    }
}

@Override
public boolean touchDown(int screenX,
                         int screenY,
                         int pointer,
                         int button) {

    boolean processed = false;

    // ignore if it's not left mouse button or first touch pointer
    if ((button == Input.Buttons.LEFT) && (pointer <= 0)) {
        // deal with HDPI monitors properly
        int realX = HdpiUtils.toBackBufferX(screenX);
        int realY = HdpiUtils.toBackBufferY(screenY);
        // NB: 2D coordinates (screenX, screenY) relative to the upper left
        // corner of the screen, with the positive x-axis pointing to the
        // right and the y-axis pointing downward. In order to be consistent
        // with the SpriteBatch.draw coordinates system, we flip y coordinate
        selectCard(realX, Gdx.graphics.getBackBufferHeight() - realY);
        processed = true;
    }

    return processed;
}
```

---

## The "You win!" message

When the player has guessed all pairs, a "You win!"-like message will be displayed. This will be implemented using a simple SVG that uses a rotated `<text>` element:

```xml
<svg version="1.1" width="512" height="512" viewBox="0 0 512 512">
    <!-- 45 degree rotation around the center, then a
         translation to vertically center the baseline -->
    <g transform="rotate(-45 256 256) translate(0 42)"
       text-anchor="middle"
       font-family="Acme" font-size="120"
       fill="white" stroke="black" stroke-width="4">
        <!-- make the text centered at (256, 256) -->
        <switch>
            <!-- German language -->
            <text x="256" y="256" systemLanguage="de">Du gewinnst!</text>
            <!-- English language -->
            <text x="256" y="256" systemLanguage="en">You win!</text>
            <!-- Spanish language -->
            <text x="256" y="256" systemLanguage="es">Tú ganas!</text>
            <!-- French language -->
            <text x="256" y="256" systemLanguage="fr">Vous gagnez!</text>
            <!-- Italian language -->
            <text x="256" y="256" systemLanguage="it">Hai vinto!</text>
            <!-- the fallback element with no systemLanguage attribute
                 if none of them match -->
            <text x="256" y="256">You win!</text>
        </switch>
    </g>
</svg>
```

| &nbsp; |
| :---: |
| *The SVG used to display a congratulation message to the player* |
{:.tbl_images .congratsSvg}

In order to display the message, an additional texture is created and updated:

```java
// SVG document containing a congratulation message
private SVGDocument congratsDoc = null;
// texture containing a congratulation message
private SVGTexture congratsTexture = null;

private void generateCongratsMessage(int screenWidth,
                                     int screenHeight) {

    // congratulation SVG is squared by design, we choose to generate a texture with
    // a size equal to 3/5 of the smallest screen dimension; e.g. on a 1920 x 1080
    // device screen, texture will have a size of (1080 * 3) / 5 = 648 pixels
    int size = (Math.min(screenWidth, screenHeight) * 3) / 5;

    // destroy previous texture, if needed
    if (congratsTexture != null) {
        congratsTexture.dispose();
    }

    congratsTexture = svg.createTexture(congratsDoc, size, size);
}

// draw the given texture by centering it on the screen
private void drawCenteredTexture(SVGTexture texture,
                                 int screenWidth,
                                 int screenHeight) {

    int texWidth = texture.getWidth();
    int texHeight = texture.getHeight();
    int texX = (screenWidth - texWidth) / 2;
    int texY = (screenHeight - texHeight) / 2;

    // draw texture at the center of screen
    batch.draw(texture,
               // destination screen region
               texX, texY, texWidth, texHeight,
               // source texture region
               0, 0, texWidth, texHeight,
               // flipX, flipY
               false, true);
}

// draw the background and the congratulation message
private void drawCongratsMessage(int screenWidth,
                                 int screenHeight) {

    // draw congratulation message at the center of screen
    drawCenteredTexture(_congratsTexture, screenWidth, screenHeight);
}

@Override
public void create() {

    ...

    // load congratulations SVG document
    congratsDoc = svg.createDocumentFromFile("gameCongrats.svg");
}

@Override
public void resize(int width,
                   int height) {

    if ((width > 0) && (height > 0)) {

        // get actual backbuffer resolution
        int screenWidth = Gdx.graphics.getBackBufferWidth();
        int screenHeight = Gdx.graphics.getBackBufferHeight();

        ...

        // generate congratulation message texture
        generateCongratsMessage(screenWidth, screenHeight);
    }
}
```

The remaining code (not reported here because really trivial) simply deals with the gameplay: if the two selected cards match, they are made inactive (`Card.active = false`) and removed from the game, otherwise they are covered again. The game ends when all the cards are inactive (i.e. all pairs of animals have been discovered).

| &nbsp; |
| :---: |
| *A new game started and we immediately chose a pair that does not match!* |
{:.tbl_images .libgdx_tut1_the_game}

---

## The project settings

As seen in the previous chapter, the SVG file used to generate the "You win!" texture message is made of `<text>` elements; all such elements inherit the use of a `Acme` font family. In addition a `<switch>` element is used to provide an ability to specify alternate viewing depending on the capabilities of a given user agent or the user's language. In the detail not all `<text>` elements will be rendered, but only the one that matches the user's language.
In order to provide AmanithSVG fonts and language settings, [SVGAssetsConfigGDX]({{site.url}}/docs/binding/003-libgdx.html#svgassetsconfiggdx) class must be used.

```java
@Override
public void create() {

    // get actual backbuffer resolution
    int screenWidth = Gdx.graphics.getBackBufferWidth();
    int screenHeight = Gdx.graphics.getBackBufferHeight();
    int deviceRotation = Gdx.input.getRotation();
    // create configuration for AmanithSVG; NB: use the backbuffer to get the real dimensions in pixels
    SVGAssetsConfigGDX cfg = new SVGAssetsConfigGDX(screenWidth, screenHeight, Gdx.graphics.getPpiX());

    // set curves quality (used by AmanithSVG geometric kernel to approximate curves with straight
    // line segments (flattening); valid range is [1; 100], where 100 represents the best quality
    cfg.setCurvesQuality(75);
    // specify the system/user-agent language; this setting will affect the conditional rendering
    // of <switch> elements and elements with 'systemLanguage' attribute specified
    cfg.setLanguage("en");
    // provide fonts, in order to render <text> elements
    cfg.addFont("acme.ttf", "Acme");

    // initialize AmanithSVG for libGDX
    svg = new SVGAssetsGDX(cfg);

    ...
}
```

In this game example we provide AmanithSVG with the `Acme` font (as needed by the congratulation message SVG). In this way, when AmanithSVG will be initialized at runtime, it will find and use the font file to render `<text>` elements.

| &nbsp; |
| :---: |
| *The Acme font* |
{:.tbl_images .acmeFont}

---

## The splash screen

The implementation of an initial splash screen, showing a "Powered by AmanithSVG" logo, is really simple. It is just a matter to load the relative SVG document, and generate a texture. Because the logo must cover the whole screen, while maintaining its original aspect ratio (i.e. the aspect ratio induced by the `viewBox` attribute), we must calculate the smallest scale factor that "fits" the logo within the screen.

```java
// splash screen SVG document
private SVGDocument splashDoc = null;
// "Powered by AmanithSVG" texture
private SVGTexture splashTexture = null;

// generate "Powered by AmanithSVG" splash screen texture
private void generateSplashScreen(int screenWidth,
                                  int screenHeight) {

    float viewW = splashDoc.getViewport().getWidth();
    float viewH = splashDoc.getViewport().getHeight();
    // keep the SVG aspect ratio
    float sx = screenWidth / viewW;
    float sy = screenHeight / viewH;
    // keep 2% border on each side
    float scaleMin = Math.min(sx, sy) * 0.96f;
    // and at the same time we fit the screen
    int texW = Math.round(viewW * scaleMin);
    int texH = Math.round(viewH * scaleMin);

    // destroy previous texture, if needed
    if (splashTexture != null) {
        splashTexture.dispose();
    }

    splashTexture = svg.createTexture(splashDoc, texW, texH);
}

// display "Powered by AmanithSVG" splash screen
private void drawSplashScreen(int screenWidth,
                              int screenHeight) {

    // draw splash screen texture at the center of screen
    drawCenteredTexture(splashTexture, screenWidth, screenHeight);
}

@Override
public void create() {

    ...

    // load splash screen SVG document
    splashDoc = svg.createDocumentFromFile("powBy_AmanithSVG_Dark.svg");
}

@Override
public void resize(int width,
                   int height) {

    if ((width > 0) && (height > 0)) {

        // get actual backbuffer resolution
        int screenWidth = Gdx.graphics.getBackBufferWidth();
        int screenHeight = Gdx.graphics.getBackBufferHeight();

        ...

        // generate "Powered by AmanithSVG" splash screen texture
        generateSplashScreen(screenWidth, screenHeight);
    }
}
```

| &nbsp; |
| :---: |
| *Powered by AmanithSVG logo (for dark backgrounds)* |
{:.tbl_images .amanithsvgPowByDark}

| &nbsp; |
| :---: |
| *Powered by AmanithSVG logo (for light backgrounds)* |
{:.tbl_images .amanithsvgPowByLight}

The complete project can be found [here](http://github.com/Mazatech/amanithsvg-sdk/tree/master/examples/libgdx/gameCards). Now you can have fun experimenting with it!

## Credits

 - Sunny countryside backgrounds (gameBkg1.svg, gameBkg2.svg, gameBkg3.svg, gameBkg4.svg) have been downloaded from: [http://xoo.me/it/template/details/6960-4-sunny-countryside-vector-scenes](http://web.archive.org/web/20150404185343/http://xoo.me/it/template/details/6960-4-sunny-countryside-vector-scenes)
 - Thanks to [Juan Pablo del Peral](juan@huertatipografica.com.ar) for the Acme font, that is covered by the [SIL Open Font License, Version 1.1](http://scripts.sil.org/OFL)

---