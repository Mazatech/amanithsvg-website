---
layout: default
title: "Introduction"
date: 2018-01-01 08:00:00 +0100
chapter: 0
categories: [desc]
---

# Introduction

AmanithSVG is a middleware for reading and rendering SVG files, available as a standalone native library and supporting bindings for other languages and engines (C#, Java, Unity); AmanithSVG is based on static features of [SVG Tiny 1.2](https://www.w3.org/TR/SVGTiny12/), plus some static features of [SVG Full 1.1](https://www.w3.org/TR/SVG/).

AmanithSVG simplifies the management of image assets for all those projects that need to run across a wide range of devices with different resolutions: using the library, the developer keeps all image files in SVG
format and render them on the target platform at runtime, at the desired resolution.

The API exposes methods for the manipulation of viewports, drawing surfaces and SVG documents: there are methods for creating drawing surfaces, load SVG documents, get their information and draw them on surfaces.

The Document Object Model (DOM) is not exposed because the library is aimed to the rendering only.

Some headlines about the library:

 * Standalone: the library does not require external libraries, it can be downloaded and used just as is

 * Cross platform: the API, and so the rendering, remains consistent across all supported platforms

 * Really fast rendering and high antialiasing quality (analytical pixel coverage)

 * Based on the robust AmanithVG SRE software rendering engine

---
