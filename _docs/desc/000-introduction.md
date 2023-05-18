---
layout: default
title: "Introduction"
date: 2018-01-01 08:00:00 +0100
chapter: 0
categories: [desc]
---

# Introduction

AmanithSVG is a middleware for reading and rendering SVG files, available as a standalone native library and supporting bindings for other languages and engines (C#, Java, Unity, libGDX); AmanithSVG is based on static features of [SVG Full 1.1](https://www.w3.org/TR/SVG/), plus some static features of [SVG Tiny 1.2](https://www.w3.org/TR/SVGTiny12/).

AmanithSVG simplifies the management of image assets for all those projects that need to run across a wide range of devices with different resolutions: using the library, the developer keeps all image files in SVG format and render them on the target platform at runtime, at the desired resolution.

The API exposes methods for the manipulation of viewports, drawing surfaces and SVG documents: there are methods for creating drawing surfaces, load SVG documents, get their information and draw them on surfaces.

The Document Object Model (DOM) is not exposed because the library is aimed to the rendering only.

Some headlines about the library:

 * Standalone: the library does not require external libraries, it can be downloaded and used just as is

 * Cross platform: the API, and so the rendering, remains consistent across all supported platforms

 * Really fast rendering and high antialiasing quality (analytical pixel coverage)

 * Based on the robust AmanithVG SRE software rendering engine

 * Open Source Components: the library does not include open source components with the exception of:
    * a modified version of PugiXML ([https://pugixml.org](https://pugixml.org)) a light-weight, simple and fast XML parser for C++ with XPath support, under MIT License.
    * C_Hashmap ([https://github.com/petewarden/c_hashmap](https://github.com/petewarden/c_hashmap)), no license attached, the author states: "There are no restriction on how you reuse this code".
    * Freetype ([http://freetype.org](http://freetype.org)), Freetype license: [http://freetype.org/license.htm](http://freetype.org/license.htm), used to load font metrics and glyphs geometry.
    * Harfbuzz ([http://harfbuzz.org](http://harfbuzz.org)), Harfbuzz license: [http://github.com/harfbuzz/harfbuzz/blob/master/COPYING](http://github.com/harfbuzz/harfbuzz/blob/master/COPYING), used to support text shaping and international languages script.
    * wcwidth() function ([https://www.cl.cam.ac.uk/~mgk25/ucs/wcwidth.c](https://www.cl.cam.ac.uk/~mgk25/ucs/wcwidth.c)), used to calculate column width of ISO 10646 (Unicode) characters, license: "Permission to use, copy, modify, and distribute this software for any purpose and without fee is hereby granted. The author disclaims all warranties with regard to this software".

---
