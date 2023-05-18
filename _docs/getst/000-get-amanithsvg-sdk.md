---
layout: default
title: "Get AmanithSVG SDK"
date: 2018-01-01 08:00:00 +0100
chapter: 0
categories: [getst]
---

# Getting started

## Install CMake

Install CMake 3.12+ tool following instructions at: [https://cmake.org/install](https://cmake.org/install/).

AmanithSVG SDK uses CMake to generate makefiles and Xcode/VStudio solutions.

___

## Download AmanithSVG SDK from Github

Create an empty directory, enter it, then use Git or checkout with SVN using the web URL:

```
git clone https://github.com/Mazatech/amanithsvg-sdk.git
```

___

## What does the AmanithSVG SDK include?

The public AmanithSVG SDK, present on Github, includes:

- the header files (`.h` file extension) to build native applications for all the supported platforms
- the binary library files (`.dll`, `.dylib`, `.so`, `.a` file extensions) of AmanithSVG Lite, for all the supported platforms.
- the SVG viewer example for Desktop (Windows, MacOS X, Linux) and mobile (iOS, Android) platforms
- the SVG to bitmap command line utility for Desktop platforms
- AmanithSVG bindings for the [libGDX](http://libgdx.com) game development framework
- AmanithSVG bindings for the [Unity](http://unity3d.com/) game engine

---

## What does the AmanithSVG SDK not include?

The public AmanithSVG SDK, present on Github, does not include:

- the binary library files (`.dll`, `.dylib`, `.so`, `.a` file extensions) of AmanithSVG Full
- the source code of any version of AmanithSVG

---

## SVG Viewer example

Within the `amanithsvg-sdk` directory, you'll find `/examples/svgViewer` folder containing a simple SVG viewer example that shows how to use AmanithSVG API on several platforms/architectures.

Use `CMake` with the following options:

### Toolchain & Platform

Choose a toolchain and platform using:

```
// Windows (Visual Studio)
// -----------------------
-DCMAKE_TOOLCHAIN_FILE=./CMake/toolchain/win_x86.cmake     // Windows x86, or
-DCMAKE_TOOLCHAIN_FILE=./CMake/toolchain/win_x86_64.cmake  // Windows x86_64

// Generators
-G "Visual Studio 17 2022" -A [platform]   // Visual Studio 2022 solution, or
-G "Visual Studio 16 2019" -A [platform]   // Visual Studio 2019 solution, or
-G "Visual Studio 15 2017 [arch]"          // Visual Studio 2017 solution, or
-G "NMake Makefiles"                       // NMake
```

```
// MacOS X (Xcode)
// ---------------
-DCMAKE_TOOLCHAIN_FILE=./CMake/toolchain/osx_ub.cmake  // Universal Binary
                                                       // (arm64, x86_64)

// Generators
-G "Xcode"           // Xcode project
```

```
// Linux X11 (gcc)
// ---------------
-DCMAKE_TOOLCHAIN_FILE=./CMake/toolchain/linux_x86.cmake     // Linux x86, or
-DCMAKE_TOOLCHAIN_FILE=./CMake/toolchain/linux_x86_64.cmake  // Linux x86_64

// Generator
-G "Unix Makefiles"  // makefiles
```

```
// iOS (Xcode)
// -----------
-DCMAKE_TOOLCHAIN_FILE=./CMake/toolchain/ios_ub.cmake  // Universal Binary
                                                       // (arm64, x86_64)

// Generator
-G "Xcode"  // Xcode project
```

---

## Some CMake examples

### Windows
```
// Windows x86_64, Visual Studio 2022 solution
<open x64 Native Tools Command Prompt for VS 2022>
cmake -DCMAKE_TOOLCHAIN_FILE=./CMake/toolchain/win_x86_64.cmake --no-warn-unused-cli -G "Visual Studio 17 2022" -A x64 .
<open the generated .sln solution>

// Windows x86_64, Visual Studio 2019 solution
<open x64 Native Tools Command Prompt for VS 2019>
cmake -DCMAKE_TOOLCHAIN_FILE=./CMake/toolchain/win_x86_64.cmake --no-warn-unused-cli -G "Visual Studio 16 2019" -A x64 .
<open the generated .sln solution>

// Windows x86, Visual Studio 2017 nmake
<open x86 Native Tools Command Prompt for VS 2017>
cmake-DCMAKE_TOOLCHAIN_FILE=./CMake/toolchain/win_x86.cmake --no-warn-unused-cli -G "NMake Makefiles" .
nmake
```

### MacOS X
```
// MacOS X, Xcode project
<open a command prompt>
cmake -DCMAKE_TOOLCHAIN_FILE=./CMake/toolchain/osx_ub.cmake --no-warn-unused-cli -G "Xcode" .
<open the generated .xcodeproj project>
```

### Linux
```
// Linux x86_64, standard Makefile
<open a command prompt>
cmake -DCMAKE_TOOLCHAIN_FILE=./CMake/toolchain/linux_x86_64.cmake --no-warn-unused-cli -G "Unix Makefiles" .
make
```

### iOS
```
// iOS, Xcode project
<open a command prompt>
cmake -DCMAKE_TOOLCHAIN_FILE=./CMake/toolchain/ios_ub.cmake --no-warn-unused-cli -G "Xcode" .
<open the generated .xcodeproj project>
```

---

## Android SVG viewer

SVG viewer for Android can be compiled directly with [Android Studio](https://developer.android.com/studio) (you don't need to use CMake).

Open project located in `/examples/svgViewer/platform/android`

---

## iOS SVG viewer

SVG viewer for iOS can be compiled using Xcode only.
Once you have generated the Xcode project (through CMake), plug your iOS device in and open the .xcodeproj file.

| &nbsp; |
| :---: |
| *Open the .xcodeproj file* |
{:.tbl_images .xcode_prj}

Then select the iOS device and check project properties.
Select a valid developer certificate for the signing process.

| &nbsp; |
| :---: |
| *Select a valid developer certificate, before* |
{:.tbl_images .dev_cert_before}

| &nbsp; |
| :---: |
| *Select a valid developer certificate, after* |
{:.tbl_images .dev_cert_after}

Select the svg_player target, in order to run it.

| &nbsp; |
| :---: |
| *Select a valid developer certificate, after* |
{:.tbl_images .select_tut_target}

---

## svg2bitmap command line utility

Within the `amanithsvg-sdk` directory, you'll find `/examples/svg2bitmap` folder containing an utility to convert SVG files to bitmaps. This tools makes use of AmanithSVG API to convert single or multiple SVG files into PNG; it supports the generation of atlas too. The `svg2bitmap` utility can be compiled for Desktop platforms as follow:

### Windows
```
// Windows x86_64, Visual Studio 2022 solution
<open x64 Native Tools Command Prompt for VS 2022>
cmake -DCMAKE_TOOLCHAIN_FILE=./CMake/toolchain/win_x86_64.cmake --no-warn-unused-cli -G "Visual Studio 17 2022" -A x64 .
<open the generated .sln solution>

// Windows x86_64, Visual Studio 2022 nmake
<open x64 Native Tools Command Prompt for VS 2022>
cmake-DCMAKE_TOOLCHAIN_FILE=./CMake/toolchain/win_x86_64.cmake --no-warn-unused-cli -G "NMake Makefiles" .
nmake
```

### MacOS X
```
// MacOS X, Xcode project
<open a command prompt>
cmake -DCMAKE_TOOLCHAIN_FILE=./CMake/toolchain/osx_ub.cmake --no-warn-unused-cli -G "Xcode" .
<open the generated .xcodeproj project>

// MacOS X, standard Makefile
<open a command prompt>
cmake -DCMAKE_TOOLCHAIN_FILE=./CMake/toolchain/osx_ub.cmake --no-warn-unused-cli -G "Unix Makefiles" .
make
```

### Linux
```
// Linux x86_64, standard Makefile
<open a command prompt>
cmake -DCMAKE_TOOLCHAIN_FILE=./CMake/toolchain/linux_x86_64.cmake --no-warn-unused-cli -G "Unix Makefiles" .
make
```

### Usage and command line options
Here is how to use `svg2bitmap` utility and the list of available options:

```
USAGE:
    svg2bitmap [OPTIONS] --input=<input path/file> --output-path=<output_path>
    svg2bitmap [OPTIONS] --atlas-input=icons.svg,1,true --atlas-output=icons_atlas,xml --output-path=<output_path>

OPTIONS:
    -h, --help                    show this help message and exit
    --dpi=<flt>                   dpi resolution, must be a positive number; default is 72
    --screen-width=<int>          screen/display width, in pixels; default is 1024
    --screen-height=<int>         screen/display width, in pixels; default is 768
    --language=<str>              user-agent language (used during the 'systemLanguage' attribute resolving); a list of languages separated by semicolon (e.g.en-US;en-GB;it;fr), default is 'en'
    --fonts-path=<str>            optional fonts path, the location where font resources are searched for
    --images-path=<str>           optional images path, the location where bitmap resources (png and jpg) are searched for
    --clear-color=<str>           clear (background) color, expressed as #RRGGBBAA; default is transparent white #FFFFFF00
    --output-width=<int>          set the output width, in pixel; a negative number will cause the value to be taken directly from the SVG file(s)
    --output-height=<int>         set the output height, in pixel; a negative number will cause the value to be taken directly from the SVG file(s)
    --scale=<flt>                 additional scale to be applied to all SVG files (also in atlas mode), must be a positive number; default is 1.0
    --rendering-quality=<int>     rendering quality, must be a number between 1 and 100 (where 100 represents the best quality)
    --filter=<str>                optional post-rendering filter, valid values are: 'none' (default), 'dilate'
    --pixel-format=<str>          pixel format of produced PNG, valid values are: 'rgba' (default), 'bgra'
    --compression-level=<int>     compression level used to generate output PNG files, must be a number between 0 and 9 (default is 6)
    --atlas-mode=<str>            atlas mode parameters: <max bitmap dimensions>, <pad between each sprite in pixels>, <constraint: pow2 / npot (default)>; e.g. '2048,1,npot'
    --atlas-output=<str>          atlas output format: <prefix>,<data format: xml (default), cocos2d, json-array, json-hash, phaser2, phaser3, pixijs, godot3-sheet, godot3-tset, libgdx, spine, code-c, code-libgdx>,<map format: array (default), hash>; e.g. 'myatlas,xml,array'
    --atlas-input=<str>           in atlas mode, add the given SVG to the packing; format is: <filename>,<scale>,<explode groups> (e.g. icons.svg,0.8,true)
    -o, --output-path=<str>       optional output path, the location where output files will be written to (default is current directory)
    -q, --quiet                   quiet mode, it disables information messages and warnings

ARGUMENTS:
    -i, --input=<str>             a list of SVG files (separated by comma), or an input path that will be scanned, looking for SVG files
```

---