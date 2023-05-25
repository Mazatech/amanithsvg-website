---
layout: default
title: "Unity - game example"
date: 2018-01-01 08:00:00 +0100
chapter: 4
categories: [tut]
---

# Unity game example

In this tutorial, we will create a simple prototype of a memory-like game, using Unity and the relative [AmanithSVG binding API]({{site.url}}/docs/binding/004-unity.html). The example explained in this chapter is implemented in the [game scene](https://github.com/Mazatech/amanithsvg-sdk/blob/master/examples/unity/Assets/SVGAssets/Scenes/game.unity)

---

## The setup

First, lets create the project structure:

 - Create a new Unity project, choosing a 2D setup

 - Select the Game view, and make sure that "Scale" slider is completely on the left (i.e. `scale = 1x`); switch back to the Scene view.

    | &nbsp; |
    | :---: |
    | *Set a unitary scene scale* |
    {:.tbl_images .unity_tut_init}

 - Because the project is a new one, it is required to copy AmanithSVG binding for Unity (`Editor`, `Plugins` and `Scripts` folders) inside the new project's `Assets` folder; so the native AmanithSVG libraries and its C# interface will be available for the project.

 - Copy the [animals.svg](https://github.com/Mazatech/amanithsvg-sdk/blob/master/examples/unity/Assets/SVGAssets/SVGFiles/animals.svg.txt), [gameBkg1.svg](https://github.com/Mazatech/amanithsvg-sdk/blob/master/examples/unity/Assets/SVGAssets/SVGFiles/gameBkg1.svg.txt), [gameBkg2.svg](https://github.com/Mazatech/amanithsvg-sdk/blob/master/examples/unity/Assets/SVGAssets/SVGFiles/gameBkg1.svg.txt), [gameBkg3.svg](https://github.com/Mazatech/amanithsvg-sdk/blob/master/examples/unity/Assets/SVGAssets/SVGFiles/gameBkg1.svg.txt), [gameBkg4.svg](https://github.com/Mazatech/amanithsvg-sdk/blob/master/examples/unity/Assets/SVGAssets/SVGFiles/gameBkg4.svg.txt) files in the project's `Assets` folder. If needed, such files will be renamed automatically by [SVGRenamerImporter]({{site.url}}/docs/binding/004-unity.html#svgrenamerimporter) script, adding an additional `.txt` extension (if not already included), so that Unity can recognize it as a [TextAsset](https://docs.unity3d.com/ScriptReference/TextAsset.html).

 - The new scene should have been already populated with an orthographic camera; make sure that camera is positioned at `(0, 0, -10)`

    | &nbsp; |
    | :---: |
    | *The Game project layout after copying all needed files* |
    {:.tbl_images .unity_tut3_copy_binding}

---

## The game background

Like we did in the [previous tutorial]({{site.url}}/docs/tut/003-tut-unity-animated-character-example), in order to setup a background that covers the whole device screen, we proceed with the following steps:

 - we add a [SVGCameraBehaviour]({{site.url}}/docs/binding/004-unity.html#svgcamerabehaviour) component to the main camera (menu `Component` → `Add`, then `Scripts` subsection): this script, at each monobehaviour `Update` call, checks for a screen resolution change and, if this event occurs, first it sets the `Camera.orthographicSize` properly, then it informs all its listeners through the `OnResize` event.

 - we create the sprite that we will use as background (menu `GameObject` → `2D Object` → `Sprite`). We rename the created object as `Background`, we position it at `(0, 0, 0)` and we set the `Order in Layer` value of the `SpriteRenderer` component to `-1`, so the background sprite will always be behind all other sprites. Now we attach a `SVGBackgroundBehaviour` script to it (menu `Component` → `Add`, then `Scripts` subsection), then set its fields:

    - drag&drop one of the four backgrounds (e.g. `gameBkg1.svg`) file to the `SVG file` property

    - check the `Sliced` checkbox, because we don't want to generate a potentially scrollable background (see the relative [documentation]({{site.url}}/docs/binding/004-unity.html#svgbackgroundbehaviour))

    - uncheck the `Generate on Start()` checkbox (we will perform the generation at runtime, when the camera `OnResize` event will inform us about a screen resolution change)

    - set `Width` and `Height` values to something valid (please set `768 x 640`, the reason will be clear in the next paragraph), just to have something visible in the editor

 - we create an "empty" `GameObject` (menu `GameObject` → `Create Empty`) that will represent our main entry point (i.e. the game state and logic), we rename the created object as `Game`, then we add a script to it (menu `Component` → `Add` → `New script`); lets call it `GameExampleBehaviour` (you can see the final full implementation [here](https://github.com/Mazatech/amanithsvg-sdk/blob/master/examples/unity/Assets/SVGAssets/Scripts/GameScene/GameBehaviour.cs)).

| &nbsp; |
| :---: |
| *The basic template of our project* |
{:.tbl_images .unity_tut3_template}

Here is `GameExampleBehaviour` basic C# layout (the code is self-explanatory):

```c#
using System;
using UnityEngine;

public class GameExampleBehaviour : MonoBehaviour {

    private void ResizeBackground(int newScreenWidth, int newScreenHeight)
    {
        // we want to cover the whole screen
        Background.SlicedWidth = newScreenWidth;
        Background.SlicedHeight = newScreenHeight;

        // generate the background texture at the desired resolution
        Background.UpdateBackground(true);
    }

    private void OnResize(int newScreenWidth, int newScreenHeight)
    {
        // resize the background
        ResizeBackground(newScreenWidth, newScreenHeight);
    }

    private void StartNewGame()
    {
        // destroy current background texture
        Background.DestroyAll(true);

        // assign a new SVG file
        int idx = (backgroundIndex % BackgroundFiles.Length);
        Background.SVGFile = BackgroundFiles[idx];

        // advance for the next background SVG
        backgroundIndex++;
    }

    // Use this for initialization
    void Start()
    {
        // start with the first background
        backgroundIndex = 0;

        // start a new game
        StartNewGame();

        // register ourself for receiving resize events
        Camera.OnResize += OnResize;
        // now fire a resize event by hand
        Camera.Resize(true);
    }
    
    // the main camera, used to intercept screen resize events
    public SVGCameraBehaviour Camera;
    // the game background
    public SVGBackgroundBehaviour Background;
    // array of usable SVG backgrounds
    public TextAsset[] BackgroundFiles;

    // the current background (i.e. the index within the BackgroundFiles array)
    [NonSerialized]
    private int backgroundIndex;
}
```

The last thing left to do is to assign the public fields, by drag&drop:

 - the camera gameobject to the `Camera` field
 
 - the background gameobject to the `Background` field

 - the four background SVG (`gameBkg1.svg`, `gameBkg2.svg`, `gameBkg3.svg`, `gameBkg4.svg`) to the `BackgroundFiles` field

| &nbsp; |
| :---: |
| *The game starts to take form: camera and the background sprite...done!* |
{:.tbl_images .unity_tut3_background}

Now if we click play, we see that the camera viewing volume is covering the whole screen and the background sprite is resized according to the screen dimensions.

---

## The cards

The characters hidden behind the cards are cute animals and we model a single card instance as a set of basic attributes (see full implementation [here](https://github.com/Mazatech/amanithsvg-sdk/blob/master/examples/unity/Assets/SVGAssets/Scripts/GameScene/GameCardBehaviour.cs)). Lets start by creating a new C# script (menu `Assets` → `Create` → `C# Script`) and call it `GameExampleCardBehaviour`:

```c#
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

        int v = (int)(UnityEngine.Random.value * (float)AnimalsCount())
              + (int)GameExampleCardType.Panda;

        return (GameExampleCardType)v;
    }

    // Get the next animal in the list
    public static GameExampleCardType NextAnimal(GameExampleCardType current) {

        int next = (((int)current + 1) % AnimalsCount()) + (int)CardType.Panda;
        return (GameExampleCardType)next;
    }

#if UNITY_EDITOR
    // Reset is called when the user hits the Reset button in the Inspector's
    // context menu or when adding the component the first time.
    // This function is only called in editor mode. Reset is most commonly used
    // to give good default values in the inspector.
    void Reset()
    {
        Active = true;
        BackSide = true;
        AnimalType = GameExampleCardType.Undefined;
        Game = null;
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
```

Animals vector graphics are defined within a single SVG file ([animals.svg](https://github.com/Mazatech/amanithsvg-sdk/blob/master/examples/unity/Assets/SVGAssets/SVGFiles/animals.svg.txt)), where each animal is a single first-level group (a `<g>` element):

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
| *animals.svg* |
{:.tbl_images .animalsSvg}

As you can see from the `animals.svg` header, it has been designed for a `768 x 640` resolution (the same that we have chosen, not by chance, for the background).
Now we want to implement a function that generates animals sprites from the given SVG file, taking care of the current screen resolution. This is easy with AmanithSVG, we make use of [SVGAtlas]({{site.url}}/docs/binding/004-unity.html#svgatlas) class. We create an atlas generator (menu `Assets` → `SVGAssets` → `Create SVG sprites atlas`), that will be used to create and pack all the sprites relative to the animals. We rename the created asset as 'animalsAtlas' just to avoid confusion. Then we can proceed with its settings:

 - drag&drop the `animals.svg` file to the region labeled with `Drag & drop SVG assets` here and check the `Separate groups` option

 - set reference width and height to `768 x 640` (the dimensions of the background): so the animals sprites size will be "compatible" with the background

 - set device test width and height to `768 x 640`, so in the editor we can simulate a device with a resolution equal to the background

 - leave the `Screen match mode` to the defalt `Match Width Or Height`

 - leave the `Match` slider to the default `0.5` value

Click the Update button and you'll see the generated animals sprites. Note that at the `768 x 640 reference resolution`, each animal sprite will have a dimension of `128 x 128`: it will ALWAYS guarantee that we can easily place them on a `4 x 3` grid (landscape layout) or on a `3 x 4` grid (portrait layout), REGARDLESS OF SCREEN RESOLUTION.

| &nbsp; |
| :---: |
| *Animals sprites have been generated from animals.svg* |
{:.tbl_images .unity_tut3_animals_atlas}

Now we instantiate a single animal sprite, and we model a game card on it:

 - choose an animal (for example the cat) and click on the relative `Instantiate` button

 - rename the created gameobject to `Card00`

 - uncheck both `Resize on Start()` and `Update transform` checkboxes (because we want to resize and position the sprite programmatically, by C# code at runtime)

 - add a `GameExampleCardBehaviour` component (menu `Component` → `Add`, then `Scripts` subsection)

 - drag&drop the `Game` gameobject to the `Game` property (so the card can access to its game)

 - add a `Box Collider 2D` component (menu `Component` → `Physics 2D` → `Box Collider 2D`), because we want to intercept mouse/touch events

| &nbsp; |
| :---: |
| *We have instantiated our first card!* |
{:.tbl_images .unity_tut3_card_instantiated}

The game will include a set of 12 cards, so all we have to do is to clone 11 times the newly created card gameobject (it doesn't matter where the clones are placed in the editor); in addition we will link the `GameExampleBehaviour` component (that is our main entry point) to the 12 cards by defining an internal array of `GameExampleCardBehaviour`:

```c#
// array of cards
public GameExampleCardBehaviour[] Cards;
// the atlas used to generate animals sprite
public SVGAtlas Atlas;
```

| &nbsp; |
| :---: |
| *Now game has supervision on backgrounds SVG, card objects and animals sprites* |
{:.tbl_images .unity_tut3_cards_cloned}

The next step is to generate animals sprites at runtime, at each camera `OnResize` event; on the same occasion we have to rearrange the cards on the screen to take account of the new resolution. We split such two functionalities in different functions, within the `GameExampleBehaviour` component:

```c#
public Sprite UpdateCardSprite(GameExampleCardBehaviour card)
{
    GameExampleCardType cardType = card.BackSide ? GameExampleCardType.BackSide 
                                                 : card.AnimalType;
    // get the sprite, given its name
    string name = GameExampleCardBehaviour.AnimalSpriteName(cardType);
    SVGRuntimeSprite data = Atlas.GetSpriteByName(name);

    // keep updated the SVGSpriteLoaderBehaviour component too
    SVGSpriteLoaderBehaviour loader = 
        card.gameObject.GetComponent<SVGSpriteLoaderBehaviour>();

    card.gameObject.GetComponent<SpriteRenderer>().sprite = data.Sprite;
    loader.SpriteReference = data.SpriteReference;

    return data.Sprite;
}

private void UpdateCardsSprites()
{
    // assign the new sprites and update colliders
    for (int i = 0; i < Cards.Length; ++i)
    {
        Sprite sprite = UpdateCardSprite(Cards[i]);
        Cards[i].GetComponent<BoxCollider2D>().size = sprite.bounds.size;
    }
}

private void ResizeCards(int newScreenWidth,
                         int newScreenHeight)
{
    // update card sprites according to the current screen resolution
    if (Atlas.UpdateRuntimeSprites(newScreenWidth, newScreenHeight,
                                   out float scale))
    {
        // assign the new sprites and update colliders
        UpdateCardsSprites();
    }
}

private void PlaceCards(int screenWidth,
                        int screenHeight)
{
    int[] cardsIndexes;
    int slotsPerRow, slotsPerColumn;
    string name = GameExampleCardBehaviour.AnimalSpriteName(GameExampleCardType.BackSide);
    SVGRuntimeSprite data = Atlas.GetSpriteByName(name);
    float cardWidth = data.Sprite.bounds.size.x;
    float cardHeight = data.Sprite.bounds.size.y;
    float worldWidth = Camera.WorldWidth;
    float worldHeight = Camera.WorldHeight;

    // check actual orientation on iOS and Android devices
    if ((Application.platform == RuntimePlatform.Android) ||
        (Application.platform == RuntimePlatform.IPhonePlayer))
    {
        // portrait orientation
        if (screenWidth <= screenHeight)
        {
            // number of card slots in each dimension
            slotsPerRow = 3;
            slotsPerColumn = 4;
            cardsIndexes = CARDS_INDEXES_NATIVE_PORTRAIT;
        }
        else
        {
            // get current (landscape) orientation
            ScreenOrientation orientation = SVGAssetsUnity.ScreenOrientation;
            // number of card slots in each dimension
            slotsPerRow = 4;
            slotsPerColumn = 3;
            invertCoord = true;
            cardsIndexes = (orientation == ScreenOrientation.LandscapeRight) ? CARDS_INDEXES_LANDSCAPE_ROT90
                                                                             : CARDS_INDEXES_LANDSCAPE_ROT270;
        }
    }
    else
    {
        // Desktop, detect orientation according to screen dimensions
        slotsPerRow = (screenWidth <= screenHeight) ? 3 : 4;
        slotsPerColumn = (screenWidth <= screenHeight) ? 4 : 3;
        cardsIndexes = (screenWidth <= screenHeight) ? CARDS_INDEXES_NATIVE_PORTRAIT
                                                     : CARDS_INDEXES_NATIVE_LANDSCAPE;
    }

    // 5% border
    float ofsX = worldWidth * 0.05f;
    float ofsY = worldHeight * 0.05f;
    float horizSeparator = ((worldWidth - (slotsPerRow * cardWidth) - (2.0f * ofsX)) 
                         / (slotsPerRow - 1));
    float vertSeparator = ((worldHeight - (slotsPerColumn * cardHeight) - (2.0f * ofsY)) 
                        / (slotsPerColumn - 1));
    int cardIdx = 0;

    for (int y = 0; y < slotsPerColumn; ++y) {
        for (int x = 0; x < slotsPerRow; ++x) {
            float posX = ofsX + (x * (cardWidth + horizSeparator)) 
                       - (worldWidth * 0.5f) + (cardWidth * 0.5f);
            float posY = ofsY + (y * (cardHeight + vertSeparator)) 
                       - (worldHeight * 0.5f) + (cardHeight * 0.5f);
            // invert coordinates, if needed
            Cards[cardsIndexes[cardIdx]].transform.position = invertCoord ? new Vector3(-posX, -posY)
                                                                          : new Vector3(posX, posY);
            cardIdx++;
        }
    }
}

private void OnResize(int newScreenWidth,
                      int newScreenHeight)
{
    // resize the background
    ResizeBackground(newScreenWidth, newScreenHeight);
    // resize animals sprites
    ResizeCards(newScreenWidth, newScreenHeight);
    // rearrange cards on the screen
    PlaceCards(newScreenWidth, newScreenHeight);
}

// cards arrangement when native device orientation is portrait
private static readonly int[] CARDS_INDEXES_NATIVE_PORTRAIT = {
    0,  1,  2,
    3,  4,  5,
    6,  7,  8,
    9, 10, 11
};
// landscape orientation, clockwise from the portrait orientation
private static readonly int[] CARDS_INDEXES_LANDSCAPE_ROT90 = {
    9, 6, 3, 0,
    10, 7, 4, 1,
    11, 8, 5, 2
};
// landscape orientation, counter-clockwise from the portrait orientation
private static readonly int[] CARDS_INDEXES_LANDSCAPE_ROT270 = {
    2, 5, 8, 11,
    1, 4, 7, 10,
    0, 3, 6,  9
};

// cards arrangement when native device orientation is landscape
private static readonly int[] CARDS_INDEXES_NATIVE_LANDSCAPE = {
    0,  1,  2, 3,
    4,  5,  6, 7,
    8,  9, 10, 11
};
```

The code is really simple, here are some notes:

 - when sprites are regenerated at runtime, due to a resolution change, the relative box colliders are update too (`UpdateCardsSprites` function)

 - if the screen has a landscape layout, the cards are arranged on 3 rows and 4 columns; on portrait layouts, instead, they are arranged on 4 rows and 3 columns (`PlaceCards` function)

The `StartNewGame` function has been modified in order to generate six pairs of animals (12 cards in total), randomly chosen among the 25 available:

```c#
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
    GameExampleCardType[] animalCouples = new GameExampleCardType[Cards.Length];
    // start with a random animal
    GameExampleCardType currentAnimal = GameExampleCardBehaviour.RandomAnimal();

    // generate animal couples
    for (int i = 0; i < (Cards.Length / 2); ++i)
    {
        animalCouples[i * 2] = currentAnimal;
        animalCouples[(i * 2) + 1] = currentAnimal;
        currentAnimal = GameExampleCardBehaviour.NextAnimal(currentAnimal);
    }

    // shuffle couples
    Shuffle(animalCouples);

    // assign cards
    for (int i = 0; i < Cards.Length; ++i)
    {
        Cards[i].AnimalType = animalCouples[i];
        ShowCard(Cards[i]);
    }

    // destroy current background texture
    Background.DestroyAll(true);
    // assign a new SVG file
    int idx = (backgroundIndex % BackgroundFiles.Length);
    Background.SVGFile = BackgroundFiles[idx];
    // advance for the next background SVG
    backgroundIndex++;
}
```

| &nbsp; |
| :---: |
| *All the graphics are perfectly sized and respect the screen layout* |
{:.tbl_images .unity_tut3_cards_shuffled}

The last detail that is missing is how we detect if a card is selected by mouse/touch actions. This is really simple, because we have already configured box colliders on cards: we just need to write the `OnMouseDown` event handler within the `GameExampleCardBehaviour` script.

```c#
void OnMouseDown()
{
    // forward the selection event to the Game
    Game.SelectCard(this);
}
```

The remaining code (not reported here because really trivial) simply deals with the gameplay: all the cards must start as backside and if the two selected cards match, they are made inactive (`GameExampleCardBehaviour.Active = false`) and removed from the game, otherwise they are covered again. The game ends when all the cards are inactive (i.e. all pairs of animals have been discovered).

| &nbsp; |
| :---: |
| *A new game and we immediately chose a pair that does not match!* |
{:.tbl_images .unity_tut3_the_game}

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

In order to display the message, an object must be added to the scene (menu `GameObject` → `Create Empty`), its default location `(0, 0, 0)` will be just fine, because it corresponds to the center of the camera frame. Then a dedicated `GameCongratsBehaviour` script must be attached to it (menu `Component` → `Add`, then `Scripts` subsection): as soon as the script is attached to the object, it checks the presence of a `SpriteRenderer` component and creates it if not present (the default `Order in Layer` property value will be `0`, that is just fine because it means that the congratulation object/sprite will be displayed upon the background).
The script takes the congratulation SVG file as input, loads it when needed and destroys it at the `OnDestroy` event. In order to generate the texture according to screen resolution, a public `Resize` method has been exposed.

```c#
void OnDestroy()
{
    if (document != null)
    {
        // release SVG document
        document.Dispose();
    }
}

public void Resize(int newScreenWidth,
                   int newScreenHeight)
{
    // load the SVG document, if needed
    if ((document == null) && (SVGFile != null))
    {
        document = SVGAssetsUnity.CreateDocument(SVGFile.text);
    }

    if (document != null)
    {
        // congratulation SVG is squared by design, we choose to generate a texture with
        // a size equal to 3/5 of the smallest screen dimension; e.g. on a 1920 x 1080
        // device screen, texture will have a size of (1080 * 3) / 5 = 648 pixels
        uint size = (uint)(Math.Min(newScreenWidth, newScreenHeight) * 3) / 5;
        // create a drawing surface
        SVGSurfaceUnity surface = SVGAssetsUnity.CreateSurface(size, size);
        // draw SVG and generate the relative texture
        Texture2D texture = surface.DrawTexture(document, SVGColor.Clear);
        // create the sprite out of the texture
        SpriteRenderer renderer = gameObject.GetComponent<SpriteRenderer>();
        renderer.sprite = SVGAssetsUnity.CreateSprite(texture, new Vector2(0.5f, 0.5f));
        // destroy the temporary drawing surface
        surface.Dispose();
    }
}

// SVG to be displayed when the player wins.
public TextAsset SVGFile = null;
// The loaded SVG document.
private SVGDocument document = null;
```

---

## The project settings

As seen in the previous chapter, the SVG file used to generate the "You win!" texture message is made of `<text>` elements; all such elements inherit the use of a Acme font family. In addition a `<switch>` element is used to provide an ability to specify alternate viewing depending on the capabilities of a given user agent or the user's language. In the detail not all `<text>` elements will be rendered, but only the one that matches the user's language.
In order to provide AmanithSVG fonts and language settings, [SVGAssetsConfigUnity]({{site.url}}/docs/binding/004-unity.html#svgassetsconfigunity) class must be used. Such class is interfaced to Unity editor through the [SVGAssetsConfigUnityScriptable](https://github.com/Mazatech/amanithsvg-sdk/blob/master/examples/unity/Assets/SVGAssets/Scripts/SVGAssets/SVGAssetsConfigUnityScriptable.cs) scriptable object: it simply ensures that configuration parameters are saved/synchronized to an external resource file (`SVGAssetsConfigUnity.asset`, located in `Assets/SVGAssets/Resources` directory).

By opening the `Edit` → `Project Settings` menu, then `SVGAssets` subsection, it is possible to edit the basic parameters (e.g. user-agent language, curves quality) and provide resources (OTF/TTF/WOFF fonts and JPEG/PNG images) to AmanithSVG. In this game example we must drag&drop the Acme font (from `Assets/SVGAssets/Resources` directory), since it is needed by the congratulation message SVG. In this way, when AmanithSVG will be initialized at runtime, it will find and use the font file to render `<text>` elements.

| &nbsp; |
| :---: |
| *Editor panel for AmanithSVG resources and configuration parameters* |
{:.tbl_images .unity_tut3_config_parameters}

| &nbsp; |
| :---: |
| *The Acme font* |
{:.tbl_images .acmeFont}

---

## The splash screen

The implementation of an initial splash screen, showing a "Powered by AmanithSVG" logo, is really simple. It is just a matter to load the relative SVG document, and generate a texture. Because the logo must cover the whole screen, while maintaining its original aspect ratio (i.e. the aspect ratio induced by the `viewBox` attribute), we must calculate the smallest scale factor that "fits" the logo within the screen.
The code has been incapsulated within a `GameSplashScreenBehaviour` class, which is very similar to the `GameCongratsBehaviour` script, the only thing that changes is the method of calculating the texture size:

```c#
void OnDestroy()
{
    if (_document != null)
    {
        // release SVG document
        document.Dispose();
    }
}

public void Resize(int newScreenWidth,
                   int newScreenHeight)
{
    // load the SVG document, if needed
    if ((_document == null) && (SVGFile != null))
    {
        _document = SVGAssetsUnity.CreateDocument(SVGFile.text);
    }

    if (_document != null)
    {
        // get 'viewBox' dimensions
        float viewW = _document.Viewport.Width;
        float viewH = _document.Viewport.Height;
        // keep the SVG aspect ratio
        float sx = newScreenWidth / viewW;
        float sy = newScreenHeight / viewH;
        // keep 2% border on each side
        float scaleMin = Math.Min(sx, sy) * 0.96f;
        // and at the same time we fit the screen
        uint texW = (uint)Math.Round(viewW * scaleMin);
        uint texH = (uint)Math.Round(viewH * scaleMin);

        // create a drawing surface
        SVGSurfaceUnity surface = SVGAssetsUnity.CreateSurface(texW, texH);
        // draw SVG and generate the relative texture
        Texture2D texture = surface.DrawTexture(document, SVGColor.Clear);
        // create the sprite out of the texture
        SpriteRenderer renderer = gameObject.GetComponent<SpriteRenderer>();
        renderer.sprite = SVGAssetsUnity.CreateSprite(texture, new Vector2(0.5f, 0.5f));
        // destroy the temporary drawing surface
        surface.Dispose();
    }
}

// The "Powered by AmanithSVG" logo.
public TextAsset SVGFile = null;
// The loaded SVG document.
private SVGDocument document = null;
```

| &nbsp; |
| :---: |
| *Powered by AmanithSVG logo (for dark backgrounds)* |
{:.tbl_images .amanithsvgPowByDark}

| &nbsp; |
| :---: |
| *Powered by AmanithSVG logo (for light backgrounds)* |
{:.tbl_images .amanithsvgPowByLight}

As it can be seen, card objects have also an [Animation](https://docs.unity3d.com/Manual/class-Animation.html) component attached, used to perform a simple rotation when shuffling or when two selected cards match.

Now you can have fun experimenting with it!

## Credits

 - Sunny countryside backgrounds (gameBkg1.svg, gameBkg2.svg, gameBkg3.svg, gameBkg4.svg) have been downloaded from: [http://xoo.me/it/template/details/6960-4-sunny-countryside-vector-scenes](http://web.archive.org/web/20150404185343/http://xoo.me/it/template/details/6960-4-sunny-countryside-vector-scenes)
 - Thanks to Juan Pablo del Peral (juan@huertatipografica.com.ar) for the Acme font, that is covered by the [SIL Open Font License, Version 1.1](http://scripts.sil.org/OFL)

---
