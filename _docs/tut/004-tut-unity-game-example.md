---
layout: default
title: "Unity - game example"
date: 2018-01-01 08:00:00 +0100
chapter: 4
categories: [tut]
---

# Unity game example

In this tutorial, we will create a simple prototype of a memory-like game, using Unity and the relative [AmanithSVG binding API]({{site.url}}/docs/binding/004-unity.html).


---

## The setup

First, lets create the project structure:

 - Create a new Unity project, choosing a 2D setup

 - Select the Game view, and make sure that "Scale" slider is completely on the left (i.e. `scale = 1x`); switch back to the Scene view.

    | ![Set a unitary scene scale]({{site.url}}/assets/images/unity_tut3_init.png) |
    | :---: |
    | *Set a unitary scene scale* |

 - Because the project is a new one, it is required to copy AmanithSVG binding for Unity (`Editor`, `Plugins` and `Scripts` folders) inside the new project's `Assets` folder; so the native AmanithSVG libraries and its C# interface will be available for the project.

 - Copy the [animals.svg](http://github.com/Mazatech/amanithsvg-bindings/blob/master/Unity/Assets/SVGAssets/SVGFiles/animals.svg.txt), [gameBkg1.svg](http://github.com/Mazatech/amanithsvg-bindings/blob/master/Unity/Assets/SVGAssets/SVGFiles/gameBkg1.svg.txt), [gameBkg2.svg](http://github.com/Mazatech/amanithsvg-bindings/blob/master/Unity/Assets/SVGAssets/SVGFiles/gameBkg1.svg.txt), [gameBkg3.svg](http://github.com/Mazatech/amanithsvg-bindings/blob/master/Unity/Assets/SVGAssets/SVGFiles/gameBkg1.svg.txt), [gameBkg4.svg](http://github.com/Mazatech/amanithsvg-bindings/blob/master/Unity/Assets/SVGAssets/SVGFiles/gameBkg4.svg.txt) files in the project's `Assets` folder. If needed, such files will be renamed automatically by [SVGRenamerImporter]({{site.url}}/docs/binding/004-unity.html#svgrenamerimporter) script, adding an additional `.txt` extension (if not already included), so that Unity can recognize it as a [TextAsset](http://docs.unity3d.com/ScriptReference/TextAsset.html).

 - The new scene should have been already populated with an orthographic camera; make sure that camera is positioned at `(0, 0, -10)`

    | ![The project layout after copying all needed files]({{site.url}}/assets/images/unity_tut3_copy_binding.png) |
    | :---: |
    | *The project layout after copying all needed files* |


--- 

## The game background

Like we did in the [previous tutorial]({{site.url}}/docs/tut/003-tut-unity-animated-character-example), in order to setup a background that covers the whole device screen, we proceed with the following steps:

 - we add a [SVGCameraBehaviour]({{site.url}}/docs/binding/004-unity.html#svgcamerabehaviour) component to the main camera (menu `Component` → `Add`, then `Scripts` subsection): this script, at each monobehaviour `Update` call, checks for a screen resolution change and, if this event occurs, first it sets the `Camera.orthographicSize` properly, then it informs all its listeners through the `OnResize` event.

 - we create the sprite that we will use as background (menu `GameObject` → `2D Object` → `Sprite`). We rename the created object as `Background`, we position it at `(0, 0, 0)` and we set the `Order in Layer` value of the `SpriteRenderer` component to `-1`, so the background sprite will always be behind all other sprites. Now we attach a `SVGBackgroundBehaviour` script to it (menu `Component` → `Add`, then `Scripts` subsection), then set its fields:
 
    - drag&drop one of the four backgrounds (e.g. `gameBkg1.svg`) file to the `SVG file` property
    
    - check the `Sliced` checkbox, because we don't want to generate a potentially scrollable background (see the relative [documentation]({{site.url}}/docs/binding/004-unity.html#svgbackgroundbehaviour))
    
    - uncheck the `Generate on Start()` checkbox (we will perform the generation at runtime, when the camera `OnResize` event will inform us about a screen resolution change)
    
    - set `Width` and `Height` values to something valid (please set `768 x 640`, the reason will be clear in the next paragraph), just to have something visible in the editor

 - we create an "empty" `GameObject` (menu `GameObject` → `Create Empty`) that will represent our main entry point (i.e. the game state and logic), we rename the created object as `Game`, then we add a script to it (menu `Component` → `Add` → `New script`); lets call it `GameExampleBehaviour` (you can see the final full implementation [here](http://github.com/Mazatech/amanithsvg-bindings/blob/master/Unity/Assets/SVGAssets/Scripts/GameScene/GameBehaviour.cs)).

| ![The basic template of our project]({{site.url}}/assets/images/unity_tut3_template.png) |
| :---: |
| *The basic template of our project* |

Here is `GameExampleBehaviour` basic C# layout (the code is self-explanatory):

```c#
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
        int idx = (this.m_BackgroundIndex % this.BackgroundFiles.Length);
        this.Background.SVGFile = this.BackgroundFiles[idx];

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
```

The last thing left to do is to assign the public fields, by drag&drop:

 - the camera gameobject to the `Camera` field
 
 - the background gameobject to the `Background` field

 - the four background SVG (`gameBkg1.svg`, `gameBkg2.svg`, `gameBkg3.svg`, `gameBkg4.svg`) to the `BackgroundFiles` field

| ![The game starts to take form: camera and the background sprite...done!]({{site.url}}/assets/images/unity_tut3_background.png) |
| :---: |
| *The game starts to take form: camera and the background sprite...done!* |

Now if we click play, we see that the camera viewing volume is covering the whole screen and the background sprite is resized according to the screen dimensions.


---

## The cards

The characters hidden behind the cards are cute animals and we model a single card instance as a set of basic attributes (see full implementation [here](http://github.com/Mazatech/amanithsvg-bindings/blob/master/Unity/Assets/SVGAssets/Scripts/GameScene/GameCardBehaviour.cs)). Lets start by creating a new C# script (menu `Assets` → `Create` → `C# Script`) and call it `GameExampleCardBehaviour`:

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
```

Animals vector graphics are defined within a single SVG file ([animals.svg](http://github.com/Mazatech/amanithsvg-bindings/blob/master/Unity/Assets/SVGAssets/SVGFiles/animals.svg.txt)), where each animal is a single first-level group (a `<g>` element):

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

As you can see from the `animals.svg` header, it has been designed for a `768 x 640` resolution (the same that we have chosen, not by chance, for the background).
Now we want to implement a function that generates animals sprites from the given SVG file, taking care of the current screen resolution. This is easy with AmanithSVG, we make use of [SVGAtlas]({{site.url}}/docs/binding/004-unity.html#svgatlas) class. We create an atlas generator (menu `Assets` → `SVGAssets` → `Create SVG sprites atlas`), that will be used to create and pack all the sprites relative to the animals. We rename the created asset as 'animalsAtlas' just to avoid confusion. Then we can proceed with its settings:

 - drag&drop the `animals.svg` file to the region labeled with `Drag & drop SVG assets` here and check the `Separate groups` option

 - set reference width and height to `768 x 640` (the dimensions of the background): so the animals sprites size will be "compatible" with the background

 - set device test width and height to `768 x 640`, so in the editor we can simulate a device with a resolution equal to the background

 - leave the `Screen match mode` to the defalt `Match Width Or Height`

 - leave the `Match` slider to the default `0.5` value

Click the Update button and you'll see the generated animals sprites. Note that at the `768 x 640 reference resolution`, each animal sprite will have a dimension of `128 x 128`: it will ALWAYS guarantee that we can easily place them on a `4 x 3` grid (landscape layout) or on a `3 x 4` grid (portrait layout), REGARDLESS OF SCREEN RESOLUTION.

| ![Animals sprites have been generated from animals.svg]({{site.url}}/assets/images/unity_tut3_animals_atlas.png) |
| :---: |
| *Animals sprites have been generated from animals.svg* |

Now we instantiate a single animal sprite, and we model a game card on it:

 - choose an animal (for example the cat) and click on the relative `Instantiate` button

 - rename the created gameobject to `Card00`

 - uncheck both `Resize on Start()` and `Update transform` checkboxes (because we want to resize and position the sprite programmatically, by C# code at runtime)

 - add a `GameExampleCardBehaviour` component (menu `Component` → `Add`, then `Scripts` subsection)
 
 - drag&drop the `Game` gameobject to the `Game` property (so the card can access to its game)

 - add a `Box Collider 2D` component (menu `Component` → `Physics 2D` → `Box Collider 2D`), because we want to intercept mouse/touch events

| ![We have instantiated our first card!]({{site.url}}/assets/images/unity_tut3_card_instantiated.png) |
| :---: |
| *We have instantiated our first card!* |

The game will include a set of 12 cards, so all we have to do is to clone 11 times the newly created card gameobject (it doesn't matter where the clones are placed in the editor); in addition we will link the `GameExampleBehaviour` component (that is our main entry point) to the 12 cards by defining an internal array of `GameExampleCardBehaviour`:

```c#
// array of cards
public GameExampleCardBehaviour[] Cards;
// the atlas used to generate animals sprite
public SVGAtlas Atlas;
```

| ![Now game has supervision on backgrounds SVG, card objects and animals sprites]({{site.url}}/assets/images/unity_tut3_cards_cloned.png) |
| :---: |
| *Now game has supervision on backgrounds SVG, card objects and animals sprites* |

The next step is to generate animals sprites at runtime, at each camera `OnResize` event; on the same occasion we have to rearrange the cards on the screen to take account of the new resolution. We split such two functionalities in different functions, within the `GameExampleBehaviour` component:

```c#
public Sprite UpdateCardSprite(GameExampleCardBehaviour card)
{
    GameExampleCardType cardType = card.BackSide ? GameExampleCardType.BackSide 
                                                 : card.AnimalType;
    // get the sprite, given its name
    string name = GameExampleCardBehaviour.AnimalSpriteName(cardType);
    SVGRuntimeSprite data = this.Atlas.GetSpriteByName(name);
    
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
    string name = GameExampleCardBehaviour.AnimalSpriteName(GameExampleCardType.BackSide);
    SVGRuntimeSprite data = this.Atlas.GetSpriteByName(name);
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

private static readonly int[] CARDS_INDEXES_PORTRAIT = { 
    0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11
};
private static readonly int[] CARDS_INDEXES_LANDSCAPE = { 
    9, 6, 3, 0, 10, 7, 4, 1, 11, 8, 5, 2 
};

```

The code is really simple, here are some notes:

 - when sprites are regenerated at runtime, due to a resolution change, the relative box colliders are update too (`UpdateCardsSprites` function)

 - if the screen has a landscape layout, the cards are arranged on 3 rows and 4 columns; on portrait layouts, instead, they are arranged on 4 rows and 3 columns (`DisposeCards` function)

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
    int idx = (this.m_BackgroundIndex % this.BackgroundFiles.Length);
    this.Background.SVGFile = this.BackgroundFiles[idx];
    // advance for the next background SVG
    this.m_BackgroundIndex++;
}
```

| ![All the graphics are perfectly sized and respect the screen layout]({{site.url}}/assets/images/unity_tut3_cards_shuffled.png) |
| :---: |
| *All the graphics are perfectly sized and respect the screen layout* |

The last detail that is missing is how we detect if a card is selected by mouse/touch actions. This is really simple, because we have already configured box colliders on cards: we just need to write the `OnMouseDown` event handler within the `GameExampleCardBehaviour` script.

```c#
void OnMouseDown()
{
    // forward the selection event to the Game
    this.Game.SelectCard(this);
}
```

The remaining code (not reported here because really trivial) simply deals with the gameplay: all the cards must start as backside and if the two selected cards match, they are made inactive (`GameExampleCardBehaviour.Active = false`) and removed from the game, otherwise they are covered again. The game ends when all the cards are inactive (i.e. all pairs of animals have been discovered).

| ![A new game and we immediately chose a pair that does not match!]({{site.url}}/assets/images/unity_tut3_the_game.png) |
| :---: |
| *A new game and we immediately chose a pair that does not match!* |

The complete project can be found [here](http://github.com/Mazatech/amanithsvg-bindings/tree/master/Unity) (the [game scene](http://github.com/Mazatech/amanithsvg-bindings/blob/master/Unity/Assets/SVGAssets/Scenes/game.unity)).
As it can be seen, card objects have also an [Animation](http://docs.unity3d.com/Manual/class-Animation.html) component attached, used to perform a simple rotation when shuffling or when two selected cards match.

Now you can have fun experimenting with it!

---
