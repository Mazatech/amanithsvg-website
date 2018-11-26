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
    
    | &nbsp; | 
    | :---: |
    | *AmanithSVG native library dependencies added to build.gradle file* |
    {:.tbl_images .libgdx_tut1_setup_3}

After these operations, the `build.gradle` file should look like [this](http://github.com/Mazatech/amanithsvg-bindings/blob/master/libGDX/gameCards/build.gradle) and the folders structure should look like the following:

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

| &nbsp; | 
| :---: |
| *The basic template of our game* |
{:.tbl_images .libgdx_tut1_basic_template}


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

| &nbsp; | 
| :---: |
| *Backgrounds used within the game* |
{:.tbl_images .libgdx_tut1_four_backgrounds}


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

| &nbsp; | 
| :---: |
| *Rendering result* |
{:.tbl_images .animalsSvg} 

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

| &nbsp; | 
| :---: |
| *Landscape layout: 3 rows and 4 columns* |
{:.tbl_images .libgdx_tut1_landscape_layout} 

| &nbsp; | 
| :---: |
| *Portrait layout: 4 rows and 3 columns* |
{:.tbl_images .libgdx_tut1_portrait_layout} 

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

| &nbsp; | 
| :---: |
| *A new game and we immediately chose a pair that does not match!* |
{:.tbl_images .libgdx_tut1_the_game} 

The complete project can be found [here](http://github.com/Mazatech/amanithsvg-bindings/tree/master/libGDX/gameCards). Now you can have fun experimenting with it!

---