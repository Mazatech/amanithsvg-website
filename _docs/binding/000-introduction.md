---
layout: default
title: "AmanithSVG bindings"
date: 2018-01-01 08:00:00 +0100
chapter: 0
categories: [binding]
---

# Introduction

AmanithSVG rendering library is written in ANSI C++, [the official SDK](http://github.com/Mazatech/amanithsvg-sdk) comes with precompiled binaries for desktop and mobile platforms, in form of dynamic/shared libraries.

To allow the development of applications written in other languages than C/C++, there are available a set of bindings for the AmanithSVG API. The bindings include C# and Java languages, as well as [Unity](https://unity3d.com/) and [libGDX](https://libgdx.com/) game engines.

In particular, the C# / Unity bindings are based on .NET PInvoke mechanism: PInvoke allows .NET code to call functions that are implemented within the AmanithSVG C/C++ native library. The Java / Android / libGDX bindings, instead, are based on Java Native Interface mechanism (also known as JNI): JNI allows Java code to call functions that are implemented within the AmanithSVG C/C++ native library.

---
