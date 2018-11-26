---
layout: default
title: "Get AmanithSVG SDK"
date: 2018-01-01 08:00:00 +0100
chapter: 0
categories: [getst]
---

# Getting started

## Install CMake

Install CMake 3.7+ tool following instructions at: [https://cmake.org/install](https://cmake.org/install/).

AmanithSVG SDK uses CMake to generate makefiles and Xcode/VStudio solutions. 

___

## Download AmanithSVG SDK from Github

Create an empty directory, enter it, then use Git or checkout with SVN using the web URL:

```
git clone https://github.com/Mazatech/amanithsvg-sdk.git
```

---

## SVG Player example

Within the `amanithsvg-sdk` directory, you'll find `/examples/svgPlayer` folder containing a simple SVG viewer example that shows how to use AmanithSVG API on several platforms/architectures.

Use `CMake` with the following options:

### Toolchain & Platform

Choose a toolchain and platform using:

```
// Windows (Visual Studio)
// -----------------------
-DCMAKE_TOOLCHAIN_FILE=./CMake/toolchain/win_x86.cmake     // Windows x86, or
-DCMAKE_TOOLCHAIN_FILE=./CMake/toolchain/win_x86_64.cmake  // Windows x86_64

// Generators 
-G "NMake Makefiles"        // NMake, or 
-G "Visual Studio 12 2013"  // Visual Studio 2013 solution, or
-G "Visual Studio 14 2015"  // Visual Studio 2015 solution, or
-G "Visual Studio 15 2017"  // Visual Studio 2017 solution 
```

```
// MacOS X (Xcode)
// ---------------
-DCMAKE_TOOLCHAIN_FILE=./CMake/toolchain/osx_ub.cmake  // Universal Binary i386, x86_64

// Generators
-G "Unix Makefiles"  // makefiles  
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
-DCMAKE_TOOLCHAIN_FILE=./CMake/toolchain/ios_ub.cmake  // UB armv7, arm64, bitcode enabled

// Generator
-G "Xcode"  // Xcode project
```

---

## Some CMake examples

```
// Windows x86_64, Visual Studio 2015 solution
<open x64 Native Tools Command Prompt for VS 2015>
cmake -DCMAKE_TOOLCHAIN_FILE=./CMake/toolchain/win_x86_64.cmake --no-warn-unused-cli -G "Visual Studio 14 2015"
<open the generated .sln solution>

// Windows x86, Visual Studio 2017 solution
<open x86 Native Tools Command Prompt for VS 2017>
cmake -DCMAKE_TOOLCHAIN_FILE=./CMake/toolchain/win_x86.cmake --no-warn-unused-cli -G "Visual Studio 15 2017"
<open the generated .sln solution>

// Windows x86_64, Visual Studio 2017 nmake
<open x64 Native Tools Command Prompt for VS 2017>
cmake-DCMAKE_TOOLCHAIN_FILE=./CMake/toolchain/win_x86_64.cmake --no-warn-unused-cli -G "NMake Makefiles"
nmake


// MacOS X, standard Makefile
<open a command prompt>
cmake -DCMAKE_TOOLCHAIN_FILE=./CMake/toolchain/osx_ub.cmake --no-warn-unused-cli -G "Unix Makefiles"
make

// MacOS X, Xcode project
<open a command prompt>
cmake -DCMAKE_TOOLCHAIN_FILE=./CMake/toolchain/osx_ub.cmake --no-warn-unused-cli -G "Xcode"
<open the generated .xcodeproj project>


// Linux x86_64, standard Makefile
<open a command prompt>
cmake -DCMAKE_TOOLCHAIN_FILE=./CMake/toolchain/linux_x86_64.cmake --no-warn-unused-cli -G "Unix Makefiles"
make


// iOS, Xcode project
<open a command prompt>
cmake -DCMAKE_TOOLCHAIN_FILE=./CMake/toolchain/ios_ub.cmake --no-warn-unused-cli -G "Xcode"
<open the generated .xcodeproj project>

```

---

## Android SVG viewer

SVG viewer for Android can be compiled directly with Android Studio (you don't need to use CMake).

Open project located in `/examples/svgPlayer/platform/android`

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
