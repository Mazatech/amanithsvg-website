---
layout: default
title: "Get AmanithSVG bindings"
date: 2018-01-01 08:00:00 +0100
chapter: 0
categories: [binding]
---

# Introduction

AmanithSVG rendering library is written in ANSI C++, [the official SDK](http://github.com/Mazatech/amanithsvg-sdk) comes with precompiled binaries for desktop and mobile platforms, in form of dynamic/shared libraries.

To allow the development of applications written in other languages than C/C++, there are available a set of [opensource bindings](http://github.com/Mazatech/amanithsvg-bindings) for the AmanithSVG API.
The bindings include C# and Java languages, as well as [Unity](https://unity3d.com/) and [libGDX](https://libgdx.badlogicgames.com/) game engines.

In particular, the C# / [Unity](https://unity3d.com/) bindings are based on .NET PInvoke mechanism: PInvoke allows .NET code to call functions that are implemented within the AmanithSVG C/C++ native library.
The Java / [libGDX](https://libgdx.badlogicgames.com/) bindings, instead, are based on Java Native Interface mechanism (also known as JNI): JNI allows Java code to call functions that are implemented within the AmanithSVG C/C++ native library.

AmanithSVG bindings have been released and are licensed under the [BSD 3-Clause License](http://spdx.org/licenses/BSD-3-Clause.html).


---

## Download AmanithSVG bindings from Github

Create an empty directory, enter it, then use Git or checkout with SVN using the web URL:

```
git clone https://github.com/Mazatech/amanithsvg-bindings.git
```

---
