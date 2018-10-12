---
layout: default
title: "SVG features compatibility"
date: 2018-01-01 08:00:00 +0100
chapter: 3
categories: [desc]
---

# SVG Features compatibility

AmanithSVG implements the rendering of static SVG files, supporting a limited set of SVG elements and
attributes, as follow. 

Index:

 - [Elements (complete list)](#elements)
	- [Document structure elements](#document-structure-elements)
	- [Styling elements](#styling-elements)
	- [Paths elements](#paths-elements)
	- [Basic shapes elements](#basic-shapes-elements)
	- [Text elements](#text-elements)
	- [Painting elements](#painting-elements)
	- [Color elements](#color-elements)
	- [Gradients and patterns elements](#gradients-and-patterns-elements)
	- [Clipping, masking and compositing elements](#clipping-masking-and-compositing-elements)
	- [Filter effects elements](#filter-effects-elements)
	- [Interactivity elements](#interactivity-elements)
	- [Linking elements](#linking-elements)
	- [Scripting elements](#scripting-elements)
	- [Animation elements](#animation-elements)
	- [Fonts elements](#fonts-elements)
	- [Metadate elements](#metadate-elements)
	- [Extensibility elements](#extensibility-elements)

 - [Attributes](#attributes)
	- [Animation event attributes](#animation-event-attributes) 
	- [Animation attribute target attributes](#animation-attribute-target-attributes) 
	- [Animation timing attributes](#animation-timing-attributes)
	- [Animation value attributes](#animation-value-attributes) 
	- [Animation addition attributes](#animation-addition-attributes) 
	- [Conditional processing attributes](#conditional-processing-attributes) 
	- [Core attributes](#core-attributes) 
	- [Document event attributes](#document-event-attributes) 
	- [Filter primitive attributes](#filter-primitive-attributes) 
	- [Graphical event attributes](#graphical-event-attributes) 
	- [Presentation attributes](#presentation-attributes) 
	- [Transfer function element attributes](#transfer-function-element-attributes) 
	- [Xlink attributes](#xlink-attributes) 

[Toggle display supported/unsupported features block](){:#features-show-hide}

---


## Elements

| Elements | SVG Full 1.1 | SVG Tiny 1.2 | AmanithSVG | Notes |
| ---------- | :----------: | :----------: | :--------: | ----- |
| [\<svg\>](#svg-attributes) | <span class="supported"></span> | <span class="supported"></span> | <span class="supported"></span> | - |
| [\<g\>](#g-attributes) | <span class="supported"></span> | <span class="supported"></span> | <span class="supported"></span> | - |
| [\<defs\>](#defs-attributes) | <span class="supported"></span> | <span class="supported"></span> | <span class="supported"></span> | - |
| [\<desc\>](#desc-attributes) | <span class="supported"></span> | <span class="supported"></span> | <span class="supported"></span> | - |
| [\<title\>](#title-attributes) | <span class="supported"></span> | <span class="supported"></span> | <span class="supported"></span> | - |
| [\<symbol\>](#symbol-attributes) | <span class="supported"></span> | <span class="unsupported"></span> | <span class="supported"></span> | - |
| [\<use\>](#use-attributes) | <span class="supported"></span> | <span class="supported"></span> | <span class="supported"></span> | - |
| [\<image\>](#image-attributes) | <span class="supported"></span> | <span class="unsupported"></span> | <span class="unsupported"></span> | - |
| [\<switch\>](#switch-attributes) | <span class="supported"></span> | <span class="supported"></span> | <span class="unsupported"></span> | - |
| [\<style\>](#style-attributes) | <span class="supported"></span> | <span class="unsupported"></span> | <span class="unsupported"></span> | - |
| [\<path\>](#path-attributes) | <span class="supported"></span> | <span class="supported"></span> | <span class="supported"></span> | - |
| [\<rect\>](#rect-attributes) | <span class="supported"></span> | <span class="supported"></span> | <span class="supported"></span> | - |
| [\<circle\>](#circle-attributes) | <span class="supported"></span> | <span class="supported"></span> | <span class="supported"></span> | - |
| [\<ellipse\>](#ellipse-attributes) | <span class="supported"></span> | <span class="supported"></span> | <span class="supported"></span> | - |
| [\<line\>](#line-attributes) | <span class="supported"></span> | <span class="supported"></span> | <span class="supported"></span> | - |
| [\<polyline\>](#polyline-attributes) | <span class="supported"></span> | <span class="supported"></span> | <span class="supported"></span> | - |
| [\<polygon\>](#polygon-attributes) | <span class="supported"></span> | <span class="supported"></span> | <span class="supported"></span> | - |
| [\<text\>](#text-attributes) | <span class="supported"></span> | <span class="supported"></span> | <span class="unsupported"></span> | - |
| [\<tspan\>](#tspan-attributes) | <span class="supported"></span> | <span class="supported"></span> | <span class="unsupported"></span> | - |
| [\<tref\>](#tref-attributes) | <span class="supported"></span> | <span class="unsupported"></span> | <span class="unsupported"></span> | - |
| [\<textPath\>](#textpath-attributes) | <span class="supported"></span> | <span class="unsupported"></span> | <span class="unsupported"></span> | - |
| [\<altGlyph\>](#altglyph-attributes) | <span class="supported"></span> | <span class="unsupported"></span> | <span class="unsupported"></span> | - |
| [\<altGlyphDef\>](#altglyphdef-attributes) | <span class="supported"></span> | <span class="unsupported"></span> | <span class="unsupported"></span> | - |
| [\<altGlyphItem\>](#altglyphitem-attributes) | <span class="supported"></span> | <span class="unsupported"></span> | <span class="unsupported"></span> | - |
| [\<glyphRef\>](#gliphref-attributes) | <span class="supported"></span> | <span class="unsupported"></span> | <span class="unsupported"></span> | - |
| [\<textArea\>](#textarea-attributes) | <span class="unsupported"></span> | <span class="supported"></span> | <span class="unsupported"></span> | - |
| [\<tbreak\>](#tbreak-attributes) | <span class="unsupported"></span> | <span class="supported"></span> | <span class="unsupported"></span> | - |
| [\<marker\>](#marker-attributes) | <span class="supported"></span> | <span class="unsupported"></span> | <span class="unsupported"></span> | - |
| [\<solidColor\>](#solidcolor-attributes) | <span class="unsupported"></span> | <span class="supported"></span> | <span class="supported"></span> | - |
| [\<color-profile\>](#color-profile-attributes) | <span class="supported"></span> | <span class="unsupported"></span> | <span class="unsupported"></span> | - |
| [\<linearGradient\>](#lineargradient-attributes) | <span class="supported"></span> | <span class="supported"></span> | <span class="supported"></span> | - |
| [\<radialGradient\>](#radialgradient-attributes) | <span class="supported"></span> | <span class="supported"></span> | <span class="supported"></span> | - |
| [\<stop\>](#stop-attributes) | <span class="supported"></span> | <span class="supported"></span> | <span class="supported"></span> | - |
| [\<pattern\>](#pattern-attributes) | <span class="supported"></span> | <span class="unsupported"></span> | <span class="unsupported"></span> | - |
| [\<clipPath\>](#clippath-attributes) | <span class="supported"></span> | <span class="unsupported"></span> | <span class="partial"></span> | Recursive clipping limited to 15 \<clipPath\> elements. For \<clipPath\> containing multiple geometry entities, it’s required that entities must be homogeneous in terms of ‘clip-rule’ and cw/ccw direction of the outlines. |
| [\<mask\>](#mask-attributes) | <span class="supported"></span> | <span class="unsupported"></span> | <span class="supported"></span> | - |
| [\<filter\>](#filter-attributes) | <span class="supported"></span> | <span class="unsupported"></span> | <span class="supported"></span> | - |
| [\<feDistantLight\>](#fedistantlight-attributes) | <span class="supported"></span> | <span class="unsupported"></span> | <span class="unsupported"></span> | - |
| [\<fePointLight\>](#fepointlight-attributes) | <span class="supported"></span> | <span class="unsupported"></span> | <span class="unsupported"></span> | - |
| [\<feSpotLight\>](#fespotlight-attributes) | <span class="supported"></span> | <span class="unsupported"></span> | <span class="unsupported"></span> | - |
| [\<feBlend\>](#feblend-attributes) | <span class="supported"></span> | <span class="unsupported"></span> | <span class="unsupported"></span> | - |
| [\<feColorMatrix\>](#fecolormatrix-attributes) | <span class="supported"></span> | <span class="unsupported"></span> | <span class="supported"></span> | - |
| [\<feComponentTransfer\>](#fecomponenttransfer-attributes) | <span class="supported"></span> | <span class="unsupported"></span> | <span class="unsupported"></span> | - |
| [\<feFuncR\>](#fefuncr-attributes) | <span class="supported"></span> | <span class="unsupported"></span> | <span class="unsupported"></span> | - |
| [\<feFuncG\>](#fefuncg-attributes) | <span class="supported"></span> | <span class="unsupported"></span> | <span class="unsupported"></span> | - |
| [\<feFuncB\>](#fefuncb-attributes) | <span class="supported"></span> | <span class="unsupported"></span> | <span class="unsupported"></span> | - |
| [\<feFuncA\>](#fefunca-attributes) | <span class="supported"></span> | <span class="unsupported"></span> | <span class="unsupported"></span> | - |
| [\<feComposite\>](#fecomposite-attributes) | <span class="supported"></span> | <span class="unsupported"></span> | <span class="unsupported"></span> | - |
| [\<feConvolveMatrix\>](#feconvolvematrix-attributes) | <span class="supported"></span> | <span class="unsupported"></span> | <span class="unsupported"></span> | - |
| [\<feDiffuseLighting\>](#fediffuselighting-attributes) | <span class="supported"></span> | <span class="unsupported"></span> | <span class="unsupported"></span> | - |
| [\<feDisplacementMap\>](#fedisplacementmap-attributes) | <span class="supported"></span> | <span class="unsupported"></span> | <span class="unsupported"></span> | - |
| [\<feFlood\>](#feflood-attributes) | <span class="supported"></span> | <span class="unsupported"></span> | <span class="unsupported"></span> | - |
| [\<feGaussianBlur\>](#fegaussianblur-attributes) | <span class="supported"></span> | <span class="unsupported"></span> | <span class="unsupported"></span> | - |
| [\<feImage\>](#feimage-attributes) | <span class="supported"></span> | <span class="unsupported"></span> | <span class="unsupported"></span> | - |
| [\<feMerge\>](#femerge-attributes) | <span class="supported"></span> | <span class="unsupported"></span> | <span class="unsupported"></span> | - |
| [\<feMergeNode\>](#femergenode-attributes) | <span class="supported"></span> | <span class="unsupported"></span> | <span class="unsupported"></span> | - |
| [\<feMorphology\>](#femorphology-attributes) | <span class="supported"></span> | <span class="unsupported"></span> | <span class="unsupported"></span> | - |
| [\<feOffset\>](#feoffset-attributes) | <span class="supported"></span> | <span class="unsupported"></span> | <span class="unsupported"></span> | - |
| [\<feSpecularLighting\>](#fespecularlighting-attributes) | <span class="supported"></span> | <span class="unsupported"></span> | <span class="unsupported"></span> | - |
| [\<feTile\>](#fetile-attributes) | <span class="supported"></span> | <span class="unsupported"></span> | <span class="unsupported"></span> | - |
| [\<feTurbulence\>](#feturbulence-attributes) | <span class="supported"></span> | <span class="unsupported"></span> | <span class="unsupported"></span> | - |
| [\<cursor\>](#cursor-attributes) | <span class="supported"></span> | <span class="unsupported"></span> | <span class="unsupported"></span> | - |
| [\<a\>](#a-attributes) | <span class="supported"></span> | <span class="supported"></span> | <span class="unsupported"></span> | - |
| [\<view\>](#view-attributes) | <span class="supported"></span> | <span class="unsupported"></span> | <span class="unsupported"></span> | - |
| [\<script\>](#script-attributes) | <span class="supported"></span> | <span class="supported"></span> | <span class="unsupported"></span> | - |
| [\<animate\>](#animate-attributes) | <span class="supported"></span> | <span class="supported"></span> | <span class="unsupported"></span> | - |
| [\<set\>](#set-attributes) | <span class="supported"></span> | <span class="supported"></span> | <span class="unsupported"></span> | - |
| [\<animateMotion\>](#animatemotion-attributes) | <span class="supported"></span> | <span class="supported"></span> | <span class="unsupported"></span> | - |
| [\<animateColor\>](#animatecolor-attributes) | <span class="supported"></span> | <span class="supported"></span> | <span class="unsupported"></span> | - |
| [\<animateTransform\>](#animatetransform-attributes) | <span class="supported"></span> | <span class="supported"></span> | <span class="unsupported"></span> | - |
| [\<mpath\>](#mpath-attributes) | <span class="supported"></span> | <span class="supported"></span> | <span class="unsupported"></span> | - |
| [\<font\>](#font-attributes) | <span class="supported"></span> | <span class="supported"></span> | <span class="unsupported"></span> | - |
| [\<glyph\>](#glyph-attributes) | <span class="supported"></span> | <span class="supported"></span> | <span class="unsupported"></span> | - |
| [\<missing-glyph\>](#missing-glyph-attributes) | <span class="supported"></span> | <span class="supported"></span> | <span class="unsupported"></span> | - |
| [\<hkern\>](#hkern-attributes) | <span class="supported"></span> | <span class="supported"></span> | <span class="unsupported"></span> | - |
| [\<vkern\>](#vkern-attributes) | <span class="supported"></span> | <span class="supported"></span> | <span class="unsupported"></span> | - |
| [\<font-face\>](#font-face-attributes) | <span class="supported"></span> | <span class="supported"></span> | <span class="unsupported"></span> | - |
| [\<font-face-src\>](#font-face-src-attributes) | <span class="supported"></span> | <span class="supported"></span> | <span class="unsupported"></span> | - |
| [\<font-face-uri\>](#font-face-uri-attributes) | <span class="supported"></span> | <span class="supported"></span> | <span class="unsupported"></span> | - |
| [\<font-face-format\>](#font-face-format-attributes) | <span class="supported"></span> | <span class="unsupported"></span> | <span class="unsupported"></span> | - |
| [\<font-face-name\>](#font-face-name-attributes) | <span class="supported"></span> | <span class="unsupported"></span> | <span class="unsupported"></span> | - |
| [\<metadata\>](#metadata-attributes) | <span class="supported"></span> | <span class="supported"></span> | <span class="unsupported"></span> | - |
| [\<foreignObject\>](#foreignobject-attributes) | <span class="supported"></span> | <span class="supported"></span> | <span class="unsupported"></span> | - |
{:.rwd-table}

---


## Attributes

### Animation event attributes
{:.unsupported-features}

| Attributes | SVG Full 1.1 | SVG Tiny 1.2 | AmanithSVG | Notes |
| ---------- | :----------: | :----------: | :--------: | ----- |
| onbegin | <span class="supported"></span> | <span class="unsupported"></span> | <span class="unsupported"></span> | - |
| onend | <span class="supported"></span> | <span class="unsupported"></span> | <span class="unsupported"></span> | - |
| onrepeat | <span class="supported"></span> | <span class="unsupported"></span> | <span class="unsupported"></span> | - |
| onload | <span class="supported"></span> | <span class="unsupported"></span> | <span class="unsupported"></span> | - |
{:.rwd-table}
{:.unsupported-features}

### Animation attribute target attributes
{:.unsupported-features}

| Attributes | SVG Full 1.1 | SVG Tiny 1.2 | AmanithSVG | Notes |
| ---------- | :----------: | :----------: | :--------: | ----- |
| attributeType | <span class="supported"></span> | <span class="supported"></span> | <span class="unsupported"></span> | - |
| attributeName | <span class="supported"></span> | <span class="supported"></span> | <span class="unsupported"></span> | - |
{:.rwd-table}
{:.unsupported-features}

### Animation timing attributes
{:.unsupported-features}

| Attributes | SVG Full 1.1 | SVG Tiny 1.2 | AmanithSVG | Notes |
| ---------- | :----------: | :----------: | :--------: | ----- |
| begin | <span class="supported"></span> | <span class="supported"></span> | <span class="unsupported"></span> | - |
| dur | <span class="supported"></span> | <span class="supported"></span> | <span class="unsupported"></span> | - |
| end | <span class="supported"></span> | <span class="supported"></span> | <span class="unsupported"></span> | - |
| min | <span class="supported"></span> | <span class="supported"></span> | <span class="unsupported"></span> | - |
| max | <span class="supported"></span> | <span class="supported"></span> | <span class="unsupported"></span> | - |
| restart | <span class="supported"></span> | <span class="supported"></span> | <span class="unsupported"></span> | - |
| repeatCount | <span class="supported"></span> | <span class="supported"></span> | <span class="unsupported"></span> | - |
| repeatDur | <span class="supported"></span> | <span class="supported"></span> | <span class="unsupported"></span> | - |
| fill | <span class="supported"></span> | <span class="supported"></span> | <span class="unsupported"></span> | - |
{:.rwd-table}
{:.unsupported-features}

### Animation value attributes
{:.unsupported-features}

| Attributes | SVG Full 1.1 | SVG Tiny 1.2 | AmanithSVG | Notes |
| ---------- | :----------: | :----------: | :--------: | ----- |
| calcMode | <span class="supported"></span> | <span class="supported"></span> | <span class="unsupported"></span> | - |
| values | <span class="supported"></span> | <span class="supported"></span> | <span class="unsupported"></span> | - |
| keyTimes | <span class="supported"></span> | <span class="supported"></span> | <span class="unsupported"></span> | - |
| keySplines | <span class="supported"></span> | <span class="supported"></span> | <span class="unsupported"></span> | - |
| from | <span class="supported"></span> | <span class="supported"></span> | <span class="unsupported"></span> | - |
| to | <span class="supported"></span> | <span class="supported"></span> | <span class="unsupported"></span> | - |
| by | <span class="supported"></span> | <span class="supported"></span> | <span class="unsupported"></span> | - |
{:.rwd-table}
{:.unsupported-features}

### Animation addition attributes
{:.unsupported-features}

| Attributes | SVG Full 1.1 | SVG Tiny 1.2 | AmanithSVG | Notes |
| ---------- | :----------: | :----------: | :--------: | ----- |
| additive | <span class="supported"></span> | <span class="supported"></span> | <span class="unsupported"></span> | - |
| accumulate | <span class="supported"></span> | <span class="supported"></span> | <span class="unsupported"></span> | - |
{:.rwd-table}
{:.unsupported-features}

### Conditional processing attributes
{:.unsupported-features}

| Attributes | SVG Full 1.1 | SVG Tiny 1.2 | AmanithSVG | Notes |
| ---------- | :----------: | :----------: | :--------: | ----- |
| requiredFeatures | <span class="supported"></span> | <span class="supported"></span> | <span class="unsupported"></span> | - |
| requiredExtensions | <span class="supported"></span> | <span class="supported"></span> | <span class="unsupported"></span> | - |
| requiredFormats | <span class="unsupported"></span> | <span class="supported"></span> | <span class="unsupported"></span> | - |
| requiredFonts | <span class="unsupported"></span> | <span class="supported"></span> | <span class="unsupported"></span> | - |
| systemLanguage | <span class="supported"></span> | <span class="supported"></span> | <span class="unsupported"></span> | - |
{:.rwd-table}
{:.unsupported-features}

### Core attributes

| Attributes | SVG Full 1.1 | SVG Tiny 1.2 | AmanithSVG | Notes |
| ---------- | :----------: | :----------: | :--------: | ----- |
| id | <span class="supported"></span> | <span class="supported"></span> | <span class="supported"></span> | - |
| xml:base | <span class="supported"></span> | <span class="supported"></span> | <span class="unsupported"></span> | - |
| xml:lang | <span class="supported"></span> | <span class="supported"></span> | <span class="unsupported"></span> | - |
| xml:space | <span class="supported"></span> | <span class="supported"></span> | <span class="unsupported"></span> | - |
| xml:id | <span class="unsupported"></span> | <span class="supported"></span> | <span class="unsupported"></span> | - |
{:.rwd-table}

### Document event attributes
{:.unsupported-features}

| Attributes | SVG Full 1.1 | SVG Tiny 1.2 | AmanithSVG | Notes |
| ---------- | :----------: | :----------: | :--------: | ----- |
| onunload | <span class="supported"></span> | <span class="unsupported"></span> | <span class="unsupported"></span> | - |
| onabort | <span class="supported"></span> | <span class="unsupported"></span> | <span class="unsupported"></span> | - |
| onerror | <span class="supported"></span> | <span class="unsupported"></span> | <span class="unsupported"></span> | - |
| onresize | <span class="supported"></span> | <span class="unsupported"></span> | <span class="unsupported"></span> | - |
| onscroll | <span class="supported"></span> | <span class="unsupported"></span> | <span class="unsupported"></span> | - |
| onzoom | <span class="supported"></span> | <span class="unsupported"></span> | <span class="unsupported"></span> | - |
{:.rwd-table}
{:.unsupported-features}

### Filter primitive attributes
{:.unsupported-features}

| Attributes | SVG Full 1.1 | SVG Tiny 1.2 | AmanithSVG | Notes |
| ---------- | :----------: | :----------: | :--------: | ----- |
| x | <span class="supported"></span> | <span class="unsupported"></span> | <span class="unsupported"></span> | - |
| y | <span class="supported"></span> | <span class="unsupported"></span> | <span class="unsupported"></span> | - |
| width | <span class="supported"></span> | <span class="unsupported"></span> | <span class="unsupported"></span> | - |
| height | <span class="supported"></span> | <span class="unsupported"></span> | <span class="unsupported"></span> | - |
| result | <span class="supported"></span> | <span class="unsupported"></span> | <span class="unsupported"></span> | - |
{:.rwd-table}
{:.unsupported-features}

### Graphical event attributes
{:.unsupported-features}

| Attributes | SVG Full 1.1 | SVG Tiny 1.2 | AmanithSVG | Notes |
| ---------- | :----------: | :----------: | :--------: | ----- |
| onfocusin | <span class="supported"></span> | <span class="unsupported"></span> | <span class="unsupported"></span> | - |
| onfocusout | <span class="supported"></span> | <span class="unsupported"></span> | <span class="unsupported"></span> | - |
| onactivate | <span class="supported"></span> | <span class="unsupported"></span> | <span class="unsupported"></span> | - |
| onclick | <span class="supported"></span> | <span class="unsupported"></span> | <span class="unsupported"></span> | - |
| onmousedown | <span class="supported"></span> | <span class="unsupported"></span> | <span class="unsupported"></span> | - |
| onmouseup | <span class="supported"></span> | <span class="unsupported"></span> | <span class="unsupported"></span> | - |
| onmouseover | <span class="supported"></span> | <span class="unsupported"></span> | <span class="unsupported"></span> | - |
| onmousemove | <span class="supported"></span> | <span class="unsupported"></span> | <span class="unsupported"></span> | - |
| onmouseout | <span class="supported"></span> | <span class="unsupported"></span> | <span class="unsupported"></span> | - |
| onload | <span class="supported"></span> | <span class="unsupported"></span> | <span class="unsupported"></span> | - |
{:.rwd-table}
{:.unsupported-features}

### Presentation attributes

| Attributes | SVG Full 1.1 | SVG Tiny 1.2 | AmanithSVG | Notes |
| ---------- | :----------: | :----------: | :--------: | ----- |
| alignment-baseline | <span class="supported"></span> | <span class="unsupported"></span> | <span class="unsupported"></span> | - |
| audio-level | <span class="unsupported"></span> | <span class="supported"></span> | <span class="unsupported"></span> | - |
| baseline-shift | <span class="supported"></span> | <span class="unsupported"></span> | <span class="unsupported"></span> | - |
| buffered-rendering | <span class="unsupported"></span> | <span class="supported"></span> | <span class="unsupported"></span> | - |
| clip | <span class="supported"></span> | <span class="unsupported"></span> | <span class="unsupported"></span> | - |
| clip-path | <span class="supported"></span> | <span class="unsupported"></span> | <span class="supported"></span> | - |
| clip-rule | <span class="supported"></span> | <span class="unsupported"></span> | <span class="supported"></span> | - |
| color | <span class="supported"></span> | <span class="supported"></span> | <span class="supported"></span> | - |
| color-interpolation | <span class="supported"></span> | <span class="unsupported"></span> | <span class="unsupported"></span> | - |
| color-interpolation-filters | <span class="supported"></span> | <span class="unsupported"></span> | <span class="unsupported"></span> | - |
| color-profile | <span class="supported"></span> | <span class="unsupported"></span> | <span class="unsupported"></span> | - |
| color-rendering | <span class="supported"></span> | <span class="supported"></span> | <span class="unsupported"></span> | - |
| cursor | <span class="supported"></span> | <span class="unsupported"></span> | <span class="unsupported"></span> | - |
| direction | <span class="supported"></span> | <span class="supported"></span> | <span class="unsupported"></span> | - |
| display | <span class="supported"></span> | <span class="supported"></span> | <span class="partial"></span> | inline, none, inherit; other values are treated as inline |
| display-align | <span class="unsupported"></span> | <span class="supported"></span> | <span class="unsupported"></span> | - |
| dominant-baseline | <span class="supported"></span> | <span class="unsupported"></span> | <span class="unsupported"></span> | - |
| enable-background | <span class="supported"></span> | <span class="unsupported"></span> | <span class="unsupported"></span> | - |
| fill | <span class="supported"></span> | <span class="supported"></span> | <span class="supported"></span> | - |
| fill-opacity | <span class="supported"></span> | <span class="supported"></span> | <span class="supported"></span> | - |
| fill-rule | <span class="supported"></span> | <span class="supported"></span> | <span class="supported"></span> | - |
| filter | <span class="supported"></span> | <span class="unsupported"></span> | <span class="supported"></span> | - |
| flood-color | <span class="supported"></span> | <span class="unsupported"></span> | <span class="unsupported"></span> | - |
| flood-opacity | <span class="supported"></span> | <span class="unsupported"></span> | <span class="unsupported"></span> | - |
| font-family | <span class="supported"></span> | <span class="supported"></span> | <span class="unsupported"></span> | - |
| font-size | <span class="supported"></span> | <span class="supported"></span> | <span class="unsupported"></span> | - |
| font-size-adjust | <span class="supported"></span> | <span class="unsupported"></span> | <span class="unsupported"></span> | - |
| font-stretch | <span class="supported"></span> | <span class="unsupported"></span> | <span class="unsupported"></span> | - |
| font-style | <span class="supported"></span> | <span class="supported"></span> | <span class="unsupported"></span> | - |
| font-variant | <span class="supported"></span> | <span class="unsupported"></span> | <span class="unsupported"></span> | - |
| font-weight | <span class="supported"></span> | <span class="supported"></span> | <span class="unsupported"></span> | - |
| glyph-orientation-horizontal | <span class="supported"></span> | <span class="unsupported"></span> | <span class="unsupported"></span> | - |
| glyph-orientation-vertical | <span class="supported"></span> | <span class="unsupported"></span> | <span class="unsupported"></span> | - |
| image-rendering | <span class="supported"></span> | <span class="supported"></span> | <span class="unsupported"></span> | - |
| kerning | <span class="supported"></span> | <span class="unsupported"></span> | <span class="unsupported"></span> | - |
| letter-spacing | <span class="supported"></span> | <span class="unsupported"></span> | <span class="unsupported"></span> | - |
| lighting-color | <span class="supported"></span> | <span class="unsupported"></span> | <span class="unsupported"></span> | - |
| line-increment | <span class="unsupported"></span> | <span class="supported"></span> | <span class="unsupported"></span> | - |
| marker-end | <span class="supported"></span> | <span class="unsupported"></span> | <span class="unsupported"></span> | - |
| marker-mid | <span class="supported"></span> | <span class="unsupported"></span> | <span class="unsupported"></span> | - |
| marker-start | <span class="supported"></span> | <span class="unsupported"></span> | <span class="unsupported"></span> | - |
| mask | <span class="supported"></span> | <span class="unsupported"></span> | <span class="supported"></span> | - |
| opacity | <span class="supported"></span> | <span class="supported"></span> | <span class="supported"></span> | - |
| overflow | <span class="supported"></span> | <span class="unsupported"></span> | <span class="unsupported"></span> | - |
| pointer-events | <span class="supported"></span> | <span class="supported"></span> | <span class="unsupported"></span> | - |
| shape-rendering | <span class="supported"></span> | <span class="supported"></span> | <span class="unsupported"></span> | - |
| solid-color | <span class="unsupported"></span> | <span class="supported"></span> | <span class="supported"></span> | - |
| solid-opacity | <span class="unsupported"></span> | <span class="supported"></span> | <span class="supported"></span> | - |
| stop-color | <span class="supported"></span> | <span class="supported"></span> | <span class="supported"></span> | - |
| stop-opacity | <span class="supported"></span> | <span class="supported"></span> | <span class="supported"></span> | - |
| stroke | <span class="supported"></span> | <span class="supported"></span> | <span class="supported"></span> | - |
| stroke-dasharray | <span class="supported"></span> | <span class="supported"></span> | <span class="supported"></span> | - |
| stroke-dashoffset | <span class="supported"></span> | <span class="supported"></span> | <span class="supported"></span> | - |
| stroke-linecap | <span class="supported"></span> | <span class="supported"></span> | <span class="supported"></span> | - |
| stroke-linejoin | <span class="supported"></span> | <span class="supported"></span> | <span class="supported"></span> | - |
| stroke-miterlimit | <span class="supported"></span> | <span class="supported"></span> | <span class="supported"></span> | - |
| stroke-opacity | <span class="supported"></span> | <span class="supported"></span> | <span class="supported"></span> | - |
| stroke-width | <span class="supported"></span> | <span class="supported"></span> | <span class="supported"></span> | - |
| text-align | <span class="unsupported"></span> | <span class="supported"></span> | <span class="unsupported"></span> | - |
| text-anchor | <span class="supported"></span> | <span class="supported"></span> | <span class="unsupported"></span> | - |
| text-decoration | <span class="supported"></span> | <span class="unsupported"></span> | <span class="unsupported"></span> | - |
| text-rendering | <span class="supported"></span> | <span class="supported"></span> | <span class="unsupported"></span> | - |
| unicode-bidi | <span class="supported"></span> | <span class="supported"></span> | <span class="unsupported"></span> | - |
| vector-effect | <span class="unsupported"></span> | <span class="supported"></span> | <span class="supported"></span> | - |
| viewport-fill | <span class="unsupported"></span> | <span class="supported"></span> | <span class="unsupported"></span> | - |
| viewport-opacity | <span class="unsupported"></span> | <span class="supported"></span> | <span class="unsupported"></span> | - |
| visibility | <span class="supported"></span> | <span class="supported"></span> | <span class="supported"></span> | - |
| word-spacing | <span class="supported"></span> | <span class="unsupported"></span> | <span class="unsupported"></span> | - |
| writing-mode | <span class="supported"></span> | <span class="unsupported"></span> | <span class="unsupported"></span> | - |
{:.rwd-table}

### Transfer function element attributes
{:.unsupported-features}

| Attributes | SVG Full 1.1 | SVG Tiny 1.2 | AmanithSVG | Notes |
| ---------- | :----------: | :----------: | :--------: | ----- |
| type | <span class="supported"></span> | <span class="unsupported"></span> | <span class="unsupported"></span> | - |
| tableValues | <span class="supported"></span> | <span class="unsupported"></span> | <span class="unsupported"></span> | - |
| slope | <span class="supported"></span> | <span class="unsupported"></span> | <span class="unsupported"></span> | - |
| intercept | <span class="supported"></span> | <span class="unsupported"></span> | <span class="unsupported"></span> | - |
| amplitude | <span class="supported"></span> | <span class="unsupported"></span> | <span class="unsupported"></span> | - |
| exponent | <span class="supported"></span> | <span class="unsupported"></span> | <span class="unsupported"></span> | - |
| offset | <span class="supported"></span> | <span class="unsupported"></span> | <span class="unsupported"></span> | - |
{:.rwd-table}
{:.unsupported-features}

### Xlink attributes

| Attributes | SVG Full 1.1 | SVG Tiny 1.2 | AmanithSVG | Notes |
| ---------- | :----------: | :----------: | :--------: | ----- |
| xlink:href | <span class="supported"></span> | <span class="supported"></span> | <span class="supported"></span> | - |
| xlink:show | <span class="supported"></span> | <span class="supported"></span> | <span class="unsupported"></span> | - |
| xlink:actuate | <span class="supported"></span> | <span class="supported"></span> | <span class="unsupported"></span> | - |
| xlink:type | <span class="supported"></span> | <span class="supported"></span> | <span class="unsupported"></span> | - |
| xlink:role | <span class="supported"></span> | <span class="supported"></span> | <span class="unsupported"></span> | - |
| xlink:arcrole | <span class="supported"></span> | <span class="supported"></span> | <span class="unsupported"></span> | - |
| xlink:title | <span class="supported"></span> | <span class="supported"></span> | <span class="unsupported"></span> | - |
{:.rwd-table}

---


## Document structure elements

### \<svg\> attributes

| Attributes | SVG Full 1.1 | SVG Tiny 1.2 | AmanithSVG | Notes |
| ---------- | :----------: | :----------: | :--------: | ----- |
| [Conditional processing attributes](#conditional-processing-attributes) | | | | - |
| [Core attributes](#core-attributes) | | | | - |
| [Document event attributes](#document-event-attributes) | | | | - |
| [Graphical event attributes](#graphical-event-attributes) | | | | - |
| [Presentation attributes](#presentation-attributes) | | | | - |
| class | <span class="supported"></span> | <span class="unsupported"></span> | <span class="unsupported"></span> | - |
| style | <span class="supported"></span> | <span class="unsupported"></span> | <span class="supported"></span> | - |
| externalResourcesRequired | <span class="supported"></span> | <span class="unsupported"></span> | <span class="unsupported"></span> | - |
| x | <span class="supported"></span> | <span class="unsupported"></span> | <span class="supported"></span> | - |
| y | <span class="supported"></span> | <span class="unsupported"></span> | <span class="supported"></span> | - |
| width | <span class="supported"></span> | <span class="supported"></span> | <span class="supported"></span> | - |
| height | <span class="supported"></span> | <span class="supported"></span> | <span class="supported"></span> | - |
| viewBox | <span class="supported"></span> | <span class="supported"></span> | <span class="supported"></span> | - |
| preserveAspectRatio | <span class="supported"></span> | <span class="supported"></span> | <span class="supported"></span> | - |
| zoomAndPan | <span class="supported"></span> | <span class="supported"></span> | <span class="unsupported"></span> | - |
| version | <span class="supported"></span> | <span class="supported"></span> | <span class="unsupported"></span> | - |
| baseProfile | <span class="supported"></span> | <span class="supported"></span> | <span class="unsupported"></span> | - |
| contentScriptType | <span class="supported"></span> | <span class="supported"></span> | <span class="unsupported"></span> | - |
| contentStyleType | <span class="supported"></span> | <span class="unsupported"></span> | <span class="unsupported"></span> | - |
| snapshotTime | <span class="unsupported"></span> | <span class="supported"></span> | <span class="unsupported"></span> | - |
| playbackOrder | <span class="unsupported"></span> | <span class="supported"></span> | <span class="unsupported"></span> | - |
| timelineBegin | <span class="unsupported"></span> | <span class="supported"></span> | <span class="unsupported"></span> | - |
| focusable | <span class="unsupported"></span> | <span class="supported"></span> | <span class="unsupported"></span> | - |
{:.rwd-table}

### \<g\> attributes

| Attributes | SVG Full 1.1 | SVG Tiny 1.2 | AmanithSVG | Notes |
| ---------- | :----------: | :----------: | :--------: | ----- |
| [Conditional processing attributes](#conditional-processing-attributes) | | | | - |
| [Core attributes](#core-attributes) | | | | - |
| [Graphical event attributes](#graphical-event-attributes) | | | | - |
| [Presentation attributes](#presentation-attributes) | | | | - |
| class | <span class="supported"></span> | <span class="unsupported"></span> | <span class="unsupported"></span> | - |
| style | <span class="supported"></span> | <span class="unsupported"></span> | <span class="supported"></span> | - |
| externalResourcesRequired | <span class="supported"></span> | <span class="unsupported"></span> | <span class="unsupported"></span> | - |
| transform | <span class="supported"></span> | <span class="supported"></span> | <span class="supported"></span> | - |
{:.rwd-table}

### \<defs\> attributes

| Attributes | SVG Full 1.1 | SVG Tiny 1.2 | AmanithSVG | Notes |
| ---------- | :----------: | :----------: | :--------: | ----- |
| [Conditional processing attributes](#conditional-processing-attributes) | | | | - |
| [Core attributes](#core-attributes) | | | | - |
| [Graphical event attributes](#graphical-event-attributes) | | | | - |
| [Presentation attributes](#presentation-attributes) | | | | - |
| class | <span class="supported"></span> | <span class="unsupported"></span> | <span class="unsupported"></span> | - |
| style | <span class="supported"></span> | <span class="unsupported"></span> | <span class="supported"></span> | - |
| externalResourcesRequired | <span class="supported"></span> | <span class="unsupported"></span> | <span class="unsupported"></span> | - |
| transform | <span class="supported"></span> | <span class="supported"></span> | <span class="supported"></span> | - |
{:.rwd-table}

### \<desc\> attributes

| Attributes | SVG Full 1.1 | SVG Tiny 1.2 | AmanithSVG | Notes |
| ---------- | :----------: | :----------: | :--------: | ----- |
| [Core attributes](#core-attributes) | | | | - |
| class | <span class="supported"></span> | <span class="unsupported"></span> | <span class="unsupported"></span> | - |
| style | <span class="supported"></span> | <span class="unsupported"></span> | <span class="supported"></span> | - |
{:.rwd-table}

### \<title\> attributes

| Attributes | SVG Full 1.1 | SVG Tiny 1.2 | AmanithSVG | Notes |
| ---------- | :----------: | :----------: | :--------: | ----- |
| [Core attributes](#core-attributes) | | | | - |
| class | <span class="supported"></span> | <span class="unsupported"></span> | <span class="unsupported"></span> | - |
| style | <span class="supported"></span> | <span class="unsupported"></span> | <span class="supported"></span> | - |
{:.rwd-table}

### \<symbol\> attributes

| Attributes | SVG Full 1.1 | SVG Tiny 1.2 | AmanithSVG | Notes |
| ---------- | :----------: | :----------: | :--------: | ----- |
| [Core attributes](#core-attributes) | | | | - |
| [Graphical event attributes](#graphical-event-attributes) | | | | - |
| [Presentation attributes](#presentation-attributes) | | | | - |
| class | <span class="supported"></span> | | <span class="unsupported"></span> | - |
| style | <span class="supported"></span> | | <span class="supported"></span> | - |
| viewBox | <span class="supported"></span> | | <span class="supported"></span> | - |
| preserveAspectRatio | <span class="supported"></span> | | <span class="supported"></span> | - |
{:.rwd-table}

### \<use\> attributes

| Attributes | SVG Full 1.1 | SVG Tiny 1.2 | AmanithSVG | Notes |
| ---------- | :----------: | :----------: | :--------: | ----- |
| [Conditional processing attributes](#conditional-processing-attributes) | | | | - |
| [Core attributes](#core-attributes) | | | | - |
| [Graphical event attributes](#graphical-event-attributes) | | | | - |
| [Presentation attributes](#presentation-attributes) | | | | - |
| [Xlink attributes](#xlink-attributes) | | | | - |
| class | <span class="supported"></span> | <span class="unsupported"></span> | <span class="unsupported"></span> | - |
| style | <span class="supported"></span> | <span class="unsupported"></span> | <span class="supported"></span> | - |
| externalResourcesRequired | <span class="supported"></span> | <span class="unsupported"></span> | <span class="unsupported"></span> | - |
| transform | <span class="supported"></span> | <span class="supported"></span> | <span class="supported"></span> | - |
| x | <span class="supported"></span> | <span class="supported"></span> | <span class="supported"></span> | - |
| y | <span class="supported"></span> | <span class="supported"></span> | <span class="supported"></span> | - |
| width | <span class="supported"></span> | <span class="unsupported"></span> | <span class="supported"></span> | - |
| height | <span class="supported"></span> | <span class="unsupported"></span> | <span class="supported"></span> | - |
| xlink:href | <span class="supported"></span> | <span class="supported"></span> | <span class="supported"></span> | - |
{:.rwd-table}

### \<image\> attributes
{:.unsupported-features}

| Attributes | SVG Full 1.1 | SVG Tiny 1.2 | AmanithSVG | Notes |
| ---------- | :----------: | :----------: | :--------: | ----- |
| [Conditional processing attributes](#conditional-processing-attributes) | | | | - |
| [Core attributes](#core-attributes) | | | | - |
| [Graphical event attributes](#graphical-event-attributes) | | | | - |
| [Presentation attributes](#presentation-attributes) | | | | - |
| [Xlink attributes](#xlink-attributes) | | | | - |
| class | <span class="supported"></span> | | | - |
| style | <span class="supported"></span> | | | - |
| externalResourcesRequired | <span class="supported"></span> | | | - |
| preserveAspectRatio | <span class="supported"></span> | | | - |
| transform | <span class="supported"></span> | | | - |
| x | <span class="supported"></span> | | | - |
| y | <span class="supported"></span> | | | - |
| width | <span class="supported"></span> | | | - |
| height | <span class="supported"></span> | | | - |
| xlink:href | <span class="supported"></span> | | | - |
{:.rwd-table}
{:.unsupported-features}

### \<switch\> attributes
{:.unsupported-features}

| Attributes | SVG Full 1.1 | SVG Tiny 1.2 | AmanithSVG | Notes |
| ---------- | :----------: | :----------: | :--------: | ----- |
| [Conditional processing attributes](#conditional-processing-attributes) | | | | - |
| [Core attributes](#core-attributes) | | | | - |
| [Graphical event attributes](#graphical-event-attributes) | | | | - |
| [Presentation attributes](#presentation-attributes) | | | | - |
| class | <span class="supported"></span> | <span class="unsupported"></span> | | - |
| style | <span class="supported"></span> | <span class="unsupported"></span> | | - |
| externalResourcesRequired | <span class="supported"></span> | <span class="unsupported"></span> | | - |
| transform | <span class="supported"></span> | <span class="supported"></span> | | - |
{:.rwd-table}
{:.unsupported-features}

---


## Styling elements

### \<style\> attributes
{:.unsupported-features}

| Attributes | SVG Full 1.1 | SVG Tiny 1.2 | AmanithSVG | Notes |
| ---------- | :----------: | :----------: | :--------: | ----- |
| [Core attributes](#core-attributes) | | | | - |
| type | <span class="supported"></span> | | | - |
| media | <span class="supported"></span> | | | - |
| title | <span class="supported"></span> | | | - |
{:.rwd-table}
{:.unsupported-features}

---


## Paths elements

### \<path\> attributes

| Attributes | SVG Full 1.1 | SVG Tiny 1.2 | AmanithSVG | Notes |
| ---------- | :----------: | :----------: | :--------: | ----- |
| [Conditional processing attributes](#conditional-processing-attributes) | | | | - |
| [Core attributes](#core-attributes) | | | | - |
| [Graphical event attributes](#graphical-event-attributes) | | | | - |
| [Presentation attributes](#presentation-attributes) | | | | - |
| class | <span class="supported"></span> | <span class="unsupported"></span> | <span class="unsupported"></span> | - |
| style | <span class="supported"></span> | <span class="unsupported"></span> | <span class="supported"></span> | - |
| externalResourcesRequired | <span class="supported"></span> | <span class="unsupported"></span> | <span class="unsupported"></span> | - |
| transform | <span class="supported"></span> | <span class="supported"></span> | <span class="supported"></span> | - |
| d | <span class="supported"></span> | <span class="supported"></span> | <span class="supported"></span> | - |
| pathLength | <span class="supported"></span> | <span class="supported"></span> | <span class="unsupported"></span> | - |
{:.rwd-table}

---


## Basic shapes elements

### \<rect\> attributes

| Attributes | SVG Full 1.1 | SVG Tiny 1.2 | AmanithSVG | Notes |
| ---------- | :----------: | :----------: | :--------: | ----- |
| [Conditional processing attributes](#conditional-processing-attributes) | | | | - |
| [Core attributes](#core-attributes) | | | | - |
| [Graphical event attributes](#graphical-event-attributes) | | | | - |
| [Presentation attributes](#presentation-attributes) | | | | - |
| class | <span class="supported"></span> | <span class="unsupported"></span> | <span class="unsupported"></span> | - |
| style | <span class="supported"></span> | <span class="unsupported"></span> | <span class="supported"></span> | - |
| externalResourcesRequired | <span class="supported"></span> | <span class="unsupported"></span> | <span class="unsupported"></span> | - |
| transform | <span class="supported"></span> | <span class="supported"></span> | <span class="supported"></span> | - |
| x | <span class="supported"></span> | <span class="supported"></span> | <span class="supported"></span> | - |
| y | <span class="supported"></span> | <span class="supported"></span> | <span class="supported"></span> | - |
| width | <span class="supported"></span> | <span class="unsupported"></span> | <span class="supported"></span> | - |
| height | <span class="supported"></span> | <span class="unsupported"></span> | <span class="supported"></span> | - |
| rx | <span class="supported"></span> | <span class="supported"></span> | <span class="supported"></span> | - |
| ry | <span class="supported"></span> | <span class="supported"></span> | <span class="supported"></span> | - |
{:.rwd-table}

### \<circle\> attributes

| Attributes | SVG Full 1.1 | SVG Tiny 1.2 | AmanithSVG | Notes |
| ---------- | :----------: | :----------: | :--------: | ----- |
| [Conditional processing attributes](#conditional-processing-attributes) | | | | - |
| [Core attributes](#core-attributes) | | | | - |
| [Graphical event attributes](#graphical-event-attributes) | | | | - |
| [Presentation attributes](#presentation-attributes) | | | | - |
| class | <span class="supported"></span> | <span class="unsupported"></span> | <span class="unsupported"></span> | - |
| style | <span class="supported"></span> | <span class="unsupported"></span> | <span class="supported"></span> | - |
| externalResourcesRequired | <span class="supported"></span> | <span class="unsupported"></span> | <span class="unsupported"></span> | - |
| transform | <span class="supported"></span> | <span class="supported"></span> | <span class="supported"></span> | - |
| cx | <span class="supported"></span> | <span class="supported"></span> | <span class="supported"></span> | - |
| cy | <span class="supported"></span> | <span class="supported"></span> | <span class="supported"></span> | - |
| r | <span class="supported"></span> | <span class="supported"></span> | <span class="supported"></span> | - |
{:.rwd-table}

### \<ellipse\> attributes

| Attributes | SVG Full 1.1 | SVG Tiny 1.2 | AmanithSVG | Notes |
| ---------- | :----------: | :----------: | :--------: | ----- |
| [Conditional processing attributes](#conditional-processing-attributes) | | | | - |
| [Core attributes](#core-attributes) | | | | - |
| [Graphical event attributes](#graphical-event-attributes) | | | | - |
| [Presentation attributes](#presentation-attributes) | | | | - |
| class | <span class="supported"></span> | <span class="unsupported"></span> | <span class="unsupported"></span> | - |
| style | <span class="supported"></span> | <span class="unsupported"></span> | <span class="supported"></span> | - |
| externalResourcesRequired | <span class="supported"></span> | <span class="unsupported"></span> | <span class="unsupported"></span> | - |
| transform | <span class="supported"></span> | <span class="supported"></span> | <span class="supported"></span> | - |
| cx | <span class="supported"></span> | <span class="supported"></span> | <span class="supported"></span> | - |
| cy | <span class="supported"></span> | <span class="supported"></span> | <span class="supported"></span> | - |
| rx | <span class="supported"></span> | <span class="supported"></span> | <span class="supported"></span> | - |
| ry | <span class="supported"></span> | <span class="supported"></span> | <span class="supported"></span> | - |
{:.rwd-table}

### \<line\> attributes

| Attributes | SVG Full 1.1 | SVG Tiny 1.2 | AmanithSVG | Notes |
| ---------- | :----------: | :----------: | :--------: | ----- |
| [Conditional processing attributes](#conditional-processing-attributes) | | | | - |
| [Core attributes](#core-attributes) | | | | - |
| [Graphical event attributes](#graphical-event-attributes) | | | | - |
| [Presentation attributes](#presentation-attributes) | | | | - |
| class | <span class="supported"></span> | <span class="unsupported"></span> | <span class="unsupported"></span> | - |
| style | <span class="supported"></span> | <span class="unsupported"></span> | <span class="supported"></span> | - |
| externalResourcesRequired | <span class="supported"></span> | <span class="unsupported"></span> | <span class="unsupported"></span> | - |
| transform | <span class="supported"></span> | <span class="supported"></span> | <span class="supported"></span> | - |
| x1 | <span class="supported"></span> | <span class="supported"></span> | <span class="supported"></span> | - |
| y1 | <span class="supported"></span> | <span class="supported"></span> | <span class="supported"></span> | - |
| x2 | <span class="supported"></span> | <span class="supported"></span> | <span class="supported"></span> | - |
| y2 | <span class="supported"></span> | <span class="supported"></span> | <span class="supported"></span> | - |
{:.rwd-table}

### \<polyline\> attributes

| Attributes | SVG Full 1.1 | SVG Tiny 1.2 | AmanithSVG | Notes |
| ---------- | :----------: | :----------: | :--------: | ----- |
| [Conditional processing attributes](#conditional-processing-attributes) | | | | - |
| [Core attributes](#core-attributes) | | | | - |
| [Graphical event attributes](#graphical-event-attributes) | | | | - |
| [Presentation attributes](#presentation-attributes) | | | | - |
| class | <span class="supported"></span> | <span class="unsupported"></span> | <span class="unsupported"></span> | - |
| style | <span class="supported"></span> | <span class="unsupported"></span> | <span class="supported"></span> | - |
| externalResourcesRequired | <span class="supported"></span> | <span class="unsupported"></span> | <span class="unsupported"></span> | - |
| transform | <span class="supported"></span> | <span class="supported"></span> | <span class="supported"></span> | - |
| points | <span class="supported"></span> | <span class="supported"></span> | <span class="supported"></span> | - |
{:.rwd-table}

### \<polygon\> attributes

| Attributes | SVG Full 1.1 | SVG Tiny 1.2 | AmanithSVG | Notes |
| ---------- | :----------: | :----------: | :--------: | ----- |
| [Conditional processing attributes](#conditional-processing-attributes) | | | | - |
| [Core attributes](#core-attributes) | | | | - |
| [Graphical event attributes](#graphical-event-attributes) | | | | - |
| [Presentation attributes](#presentation-attributes) | | | | - |
| class | <span class="supported"></span> | <span class="unsupported"></span> | <span class="unsupported"></span> | - |
| style | <span class="supported"></span> | <span class="unsupported"></span> | <span class="supported"></span> | - |
| externalResourcesRequired | <span class="supported"></span> | <span class="unsupported"></span> | <span class="unsupported"></span> | - |
| transform | <span class="supported"></span> | <span class="supported"></span> | <span class="supported"></span> | - |
| points | <span class="supported"></span> | <span class="supported"></span> | <span class="supported"></span> | - |
{:.rwd-table}

---


## Text elements

### \<text\> attributes
{:.unsupported-features}

| Attributes | SVG Full 1.1 | SVG Tiny 1.2 | AmanithSVG | Notes |
| ---------- | :----------: | :----------: | :--------: | ----- |
| [Conditional processing attributes](#conditional-processing-attributes) | | | | - |
| [Core attributes](#core-attributes) | | | | - |
| [Graphical event attributes](#graphical-event-attributes) | | | | - |
| [Presentation attributes](#presentation-attributes) | | | | - |
| class | <span class="supported"></span> | <span class="unsupported"></span> | | - |
| style | <span class="supported"></span> | <span class="unsupported"></span> | | - |
| externalResourcesRequired | <span class="supported"></span> | <span class="unsupported"></span> | | - |
| transform | <span class="supported"></span> | <span class="supported"></span> | | - |
| lenghtAdjust | <span class="supported"></span> | <span class="unsupported"></span> | | - |
| x | <span class="supported"></span> | <span class="supported"></span> | | - |
| y | <span class="supported"></span> | <span class="supported"></span> | | - |
| dx | <span class="supported"></span> | <span class="unsupported"></span> | | - |
| dy | <span class="supported"></span> | <span class="unsupported"></span> | | - |
| rotate | <span class="supported"></span> | <span class="supported"></span> | | - |
| textLenght | <span class="supported"></span> | <span class="unsupported"></span> | | - |
| editable | <span class="unsupported"></span> | <span class="supported"></span> | | - |
{:.rwd-table}
{:.unsupported-features}

### \<tspan\> attributes
{:.unsupported-features}

| Attributes | SVG Full 1.1 | SVG Tiny 1.2 | AmanithSVG | Notes |
| ---------- | :----------: | :----------: | :--------: | ----- |
| [Conditional processing attributes](#conditional-processing-attributes) | | | | - |
| [Core attributes](#core-attributes) | | | | - |
| [Graphical event attributes](#graphical-event-attributes) | | | | - |
| [Presentation attributes](#presentation-attributes) | | | | - |
| class | <span class="supported"></span> | <span class="unsupported"></span> | | - |
| style | <span class="supported"></span> | <span class="unsupported"></span> | | - |
| externalResourcesRequired | <span class="supported"></span> | <span class="unsupported"></span> | | - |
| x | <span class="supported"></span> | <span class="unsupported"></span> | | - |
| y | <span class="supported"></span> | <span class="unsupported"></span> | | - |
| dx | <span class="supported"></span> | <span class="unsupported"></span> | | - |
| dy | <span class="supported"></span> | <span class="unsupported"></span> | | - |
| rotate | <span class="supported"></span> | <span class="unsupported"></span> | | - |
| textLenght | <span class="supported"></span> | <span class="unsupported"></span> | | - |
| lenghtAdjust | <span class="supported"></span> | <span class="unsupported"></span> | | - |
{:.rwd-table}
{:.unsupported-features}
 
### \<tref\> attributes
{:.unsupported-features}

| Attributes | SVG Full 1.1 | SVG Tiny 1.2 | AmanithSVG | Notes |
| ---------- | :----------: | :----------: | :--------: | ----- |
| [Conditional processing attributes](#conditional-processing-attributes) | | | | - |
| [Core attributes](#core-attributes) | | | | - |
| [Graphical event attributes](#graphical-event-attributes) | | | | - |
| [Presentation attributes](#presentation-attributes) | | | | - |
| [Xlink attributes](#xlink-attributes) | | | | - |
| class | <span class="supported"></span> | | | - |
| style | <span class="supported"></span> | | | - |
| externalResourcesRequired | <span class="supported"></span> | | | - |
| xlink:href | <span class="supported"></span> | | | - |
{:.rwd-table}
{:.unsupported-features}

### \<textPath\> attributes
{:.unsupported-features}

| Attributes | SVG Full 1.1 | SVG Tiny 1.2 | AmanithSVG | Notes |
| ---------- | :----------: | :----------: | :--------: | ----- |
| [Conditional processing attributes](#conditional-processing-attributes) | | | | - |
| [Core attributes](#core-attributes) | | | | - |
| [Graphical event attributes](#graphical-event-attributes) | | | | - |
| [Presentation attributes](#presentation-attributes) | | | | - |
| [Xlink attributes](#xlink-attributes) | | | | - |
| class | <span class="supported"></span> | | | - |
| style | <span class="supported"></span> | | | - |
| externalResourcesRequired | <span class="supported"></span> | | | - |
| xlink:href | <span class="supported"></span> | | | - |
| startOffset | <span class="supported"></span> | | | - |
| method | <span class="supported"></span> | | | - |
| spacing | <span class="supported"></span> | | | - |
{:.rwd-table}
{:.unsupported-features}

### \<altGlyph\> attributes
{:.unsupported-features}

| Attributes | SVG Full 1.1 | SVG Tiny 1.2 | AmanithSVG | Notes |
| ---------- | :----------: | :----------: | :--------: | ----- |
| [Conditional processing attributes](#conditional-processing-attributes) | | | | - |
| [Core attributes](#core-attributes) | | | | - |
| [Graphical event attributes](#graphical-event-attributes) | | | | - |
| [Presentation attributes](#presentation-attributes) | | | | - |
| [Xlink attributes](#xlink-attributes) | | | | - |
| class | <span class="supported"></span> | | | - |
| style | <span class="supported"></span> | | | - |
| externalResourcesRequired | <span class="supported"></span> | | | - |
| x | <span class="supported"></span> | | | - |
| y | <span class="supported"></span> | | | - |
| dx | <span class="supported"></span> | | | - |
| dy | <span class="supported"></span> | | | - |
| glyphRef | <span class="supported"></span> | | | - |
| format | <span class="supported"></span> | | | - |
| rotate | <span class="supported"></span> | | | - |
| xlink:href | <span class="supported"></span> | | | - |
{:.rwd-table}
{:.unsupported-features}

### \<altGlyphDef\> attributes
{:.unsupported-features}

| Attributes | SVG Full 1.1 | SVG Tiny 1.2 | AmanithSVG | Notes |
| ---------- | :----------: | :----------: | :--------: | ----- |
| [Core attributes](#core-attributes) | | | | - |
{:.rwd-table}
{:.unsupported-features}

### \<altGlyphItem\> attributes
{:.unsupported-features}

| Attributes | SVG Full 1.1 | SVG Tiny 1.2 | AmanithSVG | Notes |
| ---------- | :----------: | :----------: | :--------: | ----- |
| [Core attributes](#core-attributes) | | | | - |
{:.rwd-table}
{:.unsupported-features}

### \<glyphRef\> attributes
{:.unsupported-features}

| Attributes | SVG Full 1.1 | SVG Tiny 1.2 | AmanithSVG | Notes |
| ---------- | :----------: | :----------: | :--------: | ----- |
| [Core attributes](#core-attributes) | | | | - |
| [Presentation attributes](#presentation-attributes) | | | | - |
| [Xlink attributes](#xlink-attributes) | | | | - |
| class | <span class="supported"></span> | | | - |
| style | <span class="supported"></span> | | | - |
| x | <span class="supported"></span> | | | - |
| y | <span class="supported"></span> | | | - |
| dx | <span class="supported"></span> | | | - |
| dy | <span class="supported"></span> | | | - |
| glyphRef | <span class="supported"></span> | | | - |
| format | <span class="supported"></span> | | | - |
| xlink:href | <span class="supported"></span> | | | - |
{:.rwd-table}
{:.unsupported-features}

### \<textArea\> attributes
{:.unsupported-features}

| Attributes | SVG Full 1.1 | SVG Tiny 1.2 | AmanithSVG | Notes |
| ---------- | :----------: | :----------: | :--------: | ----- |
| [Conditional processing attributes](#conditional-processing-attributes) | | | | - |
| [Core attributes](#core-attributes) | | | | - |
| [Graphical event attributes](#graphical-event-attributes) | | | | - |
| [Presentation attributes](#presentation-attributes) | | | | - |
| x | | <span class="supported"></span> | | - |
| y | | <span class="supported"></span> | | - |
| width | | <span class="supported"></span> | | - |
| height | | <span class="supported"></span> | | - |
| editable | | <span class="supported"></span> | | - |
| format | | <span class="supported"></span> | | - |
{:.rwd-table}
{:.unsupported-features}

### \<tbreak\> attributes
{:.unsupported-features}

| Attributes | SVG Full 1.1 | SVG Tiny 1.2 | AmanithSVG | Notes |
| ---------- | :----------: | :----------: | :--------: | ----- |
| [Conditional processing attributes](#conditional-processing-attributes) | | | | - |
| [Core attributes](#core-attributes) | | | | - |
{:.rwd-table}
{:.unsupported-features}

---

## Painting elements

### \<marker\> attributes
{:.unsupported-features}

| Attributes | SVG Full 1.1 | SVG Tiny 1.2 | AmanithSVG | Notes |
| ---------- | :----------: | :----------: | :--------: | ----- |
| [Core attributes](#core-attributes) | | | | - |
| [Presentation attributes](#presentation-attributes) | | | | - |
| class | <span class="supported"></span> | | | - |
| style | <span class="supported"></span> | | | - |
| externalResourcesRequired | <span class="supported"></span> | | | - |
| viewBox | <span class="supported"></span> | | | - |
| preserveAspectRatio | <span class="supported"></span> | | | - |
| refX | <span class="supported"></span> | | | - |
| refY | <span class="supported"></span> | | | - |
| markerUnits | <span class="supported"></span> | | | - |
| markerWidth | <span class="supported"></span> | | | - |
| markerHeight | <span class="supported"></span> | | | - |
| orient | <span class="supported"></span> | | | - |
{:.rwd-table}
{:.unsupported-features}

### \<solidColor\> attributes

| Attributes | SVG Full 1.1 | SVG Tiny 1.2 | AmanithSVG | Notes |
| ---------- | :----------: | :----------: | :--------: | ----- |
| [Core attributes](#core-attributes) | | | | - |
| [Presentation attributes](#presentation-attributes) | | | | - |
| solid-color | | <span class="supported"></span> | <span class="supported"></span> | - |
| solid-opacity | | <span class="supported"></span> | <span class="supported"></span> | - |
{:.rwd-table}

---


## Color elements

### \<color-profile\> attributes
{:.unsupported-features}

| Attributes | SVG Full 1.1 | SVG Tiny 1.2 | AmanithSVG | Notes |
| ---------- | :----------: | :----------: | :--------: | ----- |
| [Core attributes](#core-attributes) | | | | - |
| [Xlink attributes](#xlink-attributes) | | | | - |
| local | <span class="supported"></span> | | | - |
| name | <span class="supported"></span> | | | - |
| rendering-intent | <span class="supported"></span> | | | - |
| xlink:href | <span class="supported"></span> | | | - |
{:.rwd-table}
{:.unsupported-features}

---


## Gradients and patterns elements

### \<linearGradient\> attributes

| Attributes | SVG Full 1.1 | SVG Tiny 1.2 | AmanithSVG | Notes |
| ---------- | :----------: | :----------: | :--------: | ----- |
| [Core attributes](#core-attributes) | | | | - |
| [Presentation attributes](#presentation-attributes) | | | | - |
| [Xlink attributes](#xlink-attributes) | | | | - |
| class | <span class="supported"></span> | <span class="unsupported"></span> | <span class="unsupported"></span> | - |
| style | <span class="supported"></span> | <span class="unsupported"></span> | <span class="supported"></span> | - |
| externalResourcesRequired | <span class="supported"></span> | <span class="unsupported"></span> | <span class="unsupported"></span> | - |
| transform | <span class="supported"></span> | <span class="supported"></span> | <span class="supported"></span> | - |
| x1 | <span class="supported"></span> | <span class="supported"></span> | <span class="supported"></span> | - |
| y1 | <span class="supported"></span> | <span class="supported"></span> | <span class="supported"></span> | - |
| x2 | <span class="supported"></span> | <span class="supported"></span> | <span class="supported"></span> | - |
| y2 | <span class="supported"></span> | <span class="supported"></span> | <span class="supported"></span> | - |
| gradientUnits | <span class="supported"></span> | <span class="supported"></span> | <span class="supported"></span> | - |
| gradientTransform | <span class="supported"></span> | <span class="unsupported"></span> | <span class="supported"></span> | - |
| spreadMethod | <span class="supported"></span> | <span class="unsupported"></span> | <span class="supported"></span> | - |
| xlink:href | <span class="supported"></span> | <span class="unsupported"></span> | <span class="supported"></span> | - |
{:.rwd-table}

### \<radialGradient\> attributes

| Attributes | SVG Full 1.1 | SVG Tiny 1.2 | AmanithSVG | Notes |
| ---------- | :----------: | :----------: | :--------: | ----- |
| [Core attributes](#core-attributes) | | | | - |
| [Presentation attributes](#presentation-attributes) | | | | - |
| [Xlink attributes](#xlink-attributes) | | | | - |
| class | <span class="supported"></span> | <span class="unsupported"></span> | <span class="unsupported"></span> | - |
| style | <span class="supported"></span> | <span class="unsupported"></span> | <span class="supported"></span> | - |
| externalResourcesRequired | <span class="supported"></span> | <span class="unsupported"></span> | <span class="unsupported"></span> | - |
| transform | <span class="supported"></span> | <span class="supported"></span> | <span class="supported"></span> | - |
| cx | <span class="supported"></span> | <span class="supported"></span> | <span class="supported"></span> | - |
| cy | <span class="supported"></span> | <span class="supported"></span> | <span class="supported"></span> | - |
| r | <span class="supported"></span> | <span class="supported"></span> | <span class="supported"></span> | - |
| fx | <span class="supported"></span> | <span class="unsupported"></span> | <span class="supported"></span> | - |
| fy | <span class="supported"></span> | <span class="unsupported"></span> | <span class="supported"></span> | - |
| gradientUnits | <span class="supported"></span> | <span class="supported"></span> | <span class="supported"></span> | - |
| gradientTransform | <span class="supported"></span> | <span class="unsupported"></span> | <span class="supported"></span> | - |
| spreadMethod | <span class="supported"></span> | <span class="unsupported"></span> | <span class="supported"></span> | - |
| xlink:href | <span class="supported"></span> | <span class="unsupported"></span> | <span class="supported"></span> | - |
{:.rwd-table}

### \<stop\> attributes

| Attributes | SVG Full 1.1 | SVG Tiny 1.2 | AmanithSVG | Notes |
| ---------- | :----------: | :----------: | :--------: | ----- |
| [Core attributes](#core-attributes) | | | | - |
| [Presentation attributes](#presentation-attributes) | | | | - |
| class | <span class="supported"></span> | <span class="unsupported"></span> | <span class="unsupported"></span> | - |
| style | <span class="supported"></span> | <span class="unsupported"></span> | <span class="supported"></span> | - |
| offset | <span class="supported"></span> | <span class="supported"></span> | <span class="supported"></span> | - |
| stop-color | <span class="supported"></span> | <span class="supported"></span> | <span class="supported"></span> | - |
| stop-opacity | <span class="supported"></span> | <span class="supported"></span> | <span class="supported"></span> | - |
{:.rwd-table}

### \<pattern\> attributes
{:.unsupported-features}

| Attributes | SVG Full 1.1 | SVG Tiny 1.2 | AmanithSVG | Notes |
| ---------- | :----------: | :----------: | :--------: | ----- |
| [Conditional processing attributes](#conditional-processing-attributes) | | | | - |
| [Core attributes](#core-attributes) | | | | - |
| [Presentation attributes](#presentation-attributes) | | | | - |
| [Xlink attributes](#xlink-attributes) | | | | - |
| class | <span class="supported"></span> | | | - |
| style | <span class="supported"></span> | | | - |
| externalResourcesRequired | <span class="supported"></span> | | | - |
| viewBox | <span class="supported"></span> | | | - |
| preserveAspectRatio | <span class="supported"></span> | | | - |
| x | <span class="supported"></span> | | | - |
| y | <span class="supported"></span> | | | - |
| width | <span class="supported"></span> | | | - |
| height | <span class="supported"></span> | | | - |
| patternUnits | <span class="supported"></span> | |  | - |
| patternContentUnits | <span class="supported"></span> | | | - |
| patternTransform | <span class="supported"></span> | | | - |
| xlink:href | <span class="supported"></span> | | | - |
{:.rwd-table}
{:.unsupported-features}

--


## Clipping, masking and compositing elements

### \<clipPath\> attributes

| Attributes | SVG Full 1.1 | SVG Tiny 1.2 | AmanithSVG | Notes |
| ---------- | :----------: | :----------: | :--------: | ----- |
| [Conditional processing attributes](#conditional-processing-attributes) | | | | - |
| [Core attributes](#core-attributes) | | | | - |
| [Presentation attributes](#presentation-attributes) | | | | - |
| class | <span class="supported"></span> | | <span class="unsupported"></span> | - |
| style | <span class="supported"></span> | | <span class="supported"></span> | - |
| externalResourcesRequired | <span class="supported"></span> | | <span class="unsupported"></span> | - |
| transform | <span class="supported"></span> | | <span class="supported"></span> | - |
| clipPathUnits | <span class="supported"></span> | | <span class="supported"></span> | - |
{:.rwd-table}

### \<mask\> attributes

| Attributes | SVG Full 1.1 | SVG Tiny 1.2 | AmanithSVG | Notes |
| ---------- | :----------: | :----------: | :--------: | ----- |
| [Conditional processing attributes](#conditional-processing-attributes) | | | | - |
| [Core attributes](#core-attributes) | | | | - |
| [Presentation attributes](#presentation-attributes) | | | | - |
| class | <span class="supported"></span> | | <span class="unsupported"></span> | - |
| style | <span class="supported"></span> | | <span class="supported"></span> | - |
| externalResourcesRequired | <span class="supported"></span> | | <span class="unsupported"></span> | - |
| x | <span class="supported"></span> | | <span class="supported"></span> | - |
| y | <span class="supported"></span> | | <span class="supported"></span> | - |
| width | <span class="supported"></span> | | <span class="supported"></span> | - |
| height | <span class="supported"></span> | | <span class="supported"></span> | - |
| maskUnits | <span class="supported"></span> | | <span class="supported"></span> | - |
| maskContentUnits | <span class="supported"></span> | | <span class="supported"></span> | - |
{:.rwd-table}

---


## Filter effects elements

### \<filter\> attributes

| Attributes | SVG Full 1.1 | SVG Tiny 1.2 | AmanithSVG | Notes |
| ---------- | :----------: | :----------: | :--------: | ----- |
| [Core attributes](#core-attributes) | | | | - |
| [Presentation attributes](#presentation-attributes) | | | | - |
| [Xlink attributes](#xlink-attributes) | | | | - |
| class | <span class="supported"></span> | | <span class="unsupported"></span> | - |
| style | <span class="supported"></span> | | <span class="supported"></span> | - |
| externalResourcesRequired | <span class="supported"></span> | | <span class="unsupported"></span> | - |
| x | <span class="supported"></span> | | <span class="supported"></span> | - |
| y | <span class="supported"></span> | | <span class="supported"></span> | - |
| width | <span class="supported"></span> | | <span class="supported"></span> | - |
| height | <span class="supported"></span> | | <span class="supported"></span> | - |
| filterRes | <span class="supported"></span> | | <span class="unsupported"></span> | - |
| filterUnits | <span class="supported"></span> | | <span class="unsupported"></span> | - |
| primitiveUnits | <span class="supported"></span> | | <span class="unsupported"></span> | - |
| xlink:href | <span class="supported"></span> | | <span class="unsupported"></span> | - |
{:.rwd-table}

### \<feDistantLight\> attributes
{:.unsupported-features}

| Attributes | SVG Full 1.1 | SVG Tiny 1.2 | AmanithSVG | Notes |
| ---------- | :----------: | :----------: | :--------: | ----- |
| [Core attributes](#core-attributes) | | | | - |
| azimuth | <span class="supported"></span> | | | - |
| elevation | <span class="supported"></span> | | | - |
{:.rwd-table}
{:.unsupported-features}

### \<fePointLight\> attributes
{:.unsupported-features}

| Attributes | SVG Full 1.1 | SVG Tiny 1.2 | AmanithSVG | Notes |
| ---------- | :----------: | :----------: | :--------: | ----- |
| [Core attributes](#core-attributes) | | | | - |
| x | <span class="supported"></span> | | | - |
| y | <span class="supported"></span> | | | - |
| z | <span class="supported"></span> | | | - |
{:.rwd-table}
{:.unsupported-features}

### \<feSpotLight\> attributes
{:.unsupported-features}

| Attributes | SVG Full 1.1 | SVG Tiny 1.2 | AmanithSVG | Notes |
| ---------- | :----------: | :----------: | :--------: | ----- |
| [Core attributes](#core-attributes) | | | | - |
| x | <span class="supported"></span> | | | - |
| y | <span class="supported"></span> | | | - |
| z | <span class="supported"></span> | | | - |
| pointsAtX | <span class="supported"></span> | | | - |
| pointsAtY | <span class="supported"></span> | | | - |
| pointsAtZ | <span class="supported"></span> | | | - |
| specularExponent | <span class="supported"></span> | | | - |
| limitingConeAngle | <span class="supported"></span> | | | - |
{:.rwd-table}
{:.unsupported-features}

### \<feBlend\> attributes
{:.unsupported-features}

| Attributes | SVG Full 1.1 | SVG Tiny 1.2 | AmanithSVG | Notes |
| ---------- | :----------: | :----------: | :--------: | ----- |
| [Core attributes](#core-attributes) | | | | - |
| [Presentation attributes](#presentation-attributes) | | | | - |
| [Filter primitive attributes](#filter-primitive-attributes) | | | | - |
| class | <span class="supported"></span> | | | - |
| style | <span class="supported"></span> | | | - |
| in | <span class="supported"></span> | | | - |
| in2 | <span class="supported"></span> | | | - |
| mode | <span class="supported"></span> | | | - |
{:.rwd-table}
{:.unsupported-features}

### \<feColorMatrix\> attributes

| Attributes | SVG Full 1.1 | SVG Tiny 1.2 | AmanithSVG | Notes |
| ---------- | :----------: | :----------: | :--------: | ----- |
| [Core attributes](#core-attributes) | | | | - |
| [Presentation attributes](#presentation-attributes) | | | | - |
| [Filter primitive attributes](#filter-primitive-attributes) | | | | - |
| class | <span class="supported"></span> | | <span class="unsupported"></span> | - |
| style | <span class="supported"></span> | | <span class="supported"></span> | - |
| in | <span class="supported"></span> | | <span class="partial"></span> | SourceGraphic only |
| type | <span class="supported"></span> | | <span class="supported"></span> | - |
| values | <span class="supported"></span> | | <span class="supported"></span> | - |
{:.rwd-table}

### \<feComponentTransfer\> attributes
{:.unsupported-features}

| Attributes | SVG Full 1.1 | SVG Tiny 1.2 | AmanithSVG | Notes |
| ---------- | :----------: | :----------: | :--------: | ----- |
| [Core attributes](#core-attributes) | | | | - |
| [Presentation attributes](#presentation-attributes) | | | | - |
| [Filter primitive attributes](#filter-primitive-attributes) | | | | - |
| class | <span class="supported"></span> | | | - |
| style | <span class="supported"></span> | | | - |
| in | <span class="supported"></span> | | | - |
{:.rwd-table}
{:.unsupported-features}

### \<feFuncR\> attributes
{:.unsupported-features}

| Attributes | SVG Full 1.1 | SVG Tiny 1.2 | AmanithSVG | Notes |
| ---------- | :----------: | :----------: | :--------: | ----- |
| [Core attributes](#core-attributes) | | | | - |
| [Transfer function element attributes](#transfer-function-element-attributes) | | | | - |
| type | <span class="supported"></span> | | | - |
| tableValues | <span class="supported"></span> | | | - |
| slope | <span class="supported"></span> | | | - |
| intercept | <span class="supported"></span> | | | - |
| amplitude | <span class="supported"></span> | | | - |
| exponent | <span class="supported"></span> | | | - |
| offset | <span class="supported"></span> | | | - |
{:.rwd-table}
{:.unsupported-features}

### \<feFuncG\> attributes
{:.unsupported-features}

| Attributes | SVG Full 1.1 | SVG Tiny 1.2 | AmanithSVG | Notes |
| ---------- | :----------: | :----------: | :--------: | ----- |
| [Core attributes](#core-attributes) | | | | - |
| [Transfer function element attributes](#transfer-function-element-attributes) | | | | - |
| type | <span class="supported"></span> | | | - |
| tableValues | <span class="supported"></span> | | | - |
| slope | <span class="supported"></span> | | | - |
| intercept | <span class="supported"></span> | | | - |
| amplitude | <span class="supported"></span> | | | - |
| exponent | <span class="supported"></span> | | | - |
| offset | <span class="supported"></span> | | | - |
{:.rwd-table}
{:.unsupported-features}

### \<feFuncB\> attributes
{:.unsupported-features}

| Attributes | SVG Full 1.1 | SVG Tiny 1.2 | AmanithSVG | Notes |
| ---------- | :----------: | :----------: | :--------: | ----- |
| [Core attributes](#core-attributes) | | | | - |
| [Transfer function element attributes](#transfer-function-element-attributes) | | | | - |
| type | <span class="supported"></span> | | | - |
| tableValues | <span class="supported"></span> | | | - |
| slope | <span class="supported"></span> | | | - |
| intercept | <span class="supported"></span> | | | - |
| amplitude | <span class="supported"></span> | | | - |
| exponent | <span class="supported"></span> | | | - |
| offset | <span class="supported"></span> | | | - |
{:.rwd-table}
{:.unsupported-features}

### \<feFuncA\> attributes
{:.unsupported-features}

| Attributes | SVG Full 1.1 | SVG Tiny 1.2 | AmanithSVG | Notes |
| ---------- | :----------: | :----------: | :--------: | ----- |
| [Core attributes](#core-attributes) | | | | - |
| [Transfer function element attributes](#transfer-function-element-attributes) | | | | - |
| type | <span class="supported"></span> | | | - |
| tableValues | <span class="supported"></span> | | | - |
| slope | <span class="supported"></span> | | | - |
| intercept | <span class="supported"></span> | | | - |
| amplitude | <span class="supported"></span> | | | - |
| exponent | <span class="supported"></span> | | | - |
| offset | <span class="supported"></span> | | | - |
{:.rwd-table}
{:.unsupported-features}

### \<feComposite\> attributes
{:.unsupported-features}

| Attributes | SVG Full 1.1 | SVG Tiny 1.2 | AmanithSVG | Notes |
| ---------- | :----------: | :----------: | :--------: | ----- |
| [Core attributes](#core-attributes) | | | | - |
| [Presentation attributes](#presentation-attributes) | | | | - |
| [Filter primitive attributes](#filter-primitive-attributes) | | | | - |
| class | <span class="supported"></span> | | | - |
| style | <span class="supported"></span> | | | - |
| in | <span class="supported"></span> | | | - |
| in2 | <span class="supported"></span> | | | - |
| operator | <span class="supported"></span> | | | - |
| k1 | <span class="supported"></span> | | | - |
| k2 | <span class="supported"></span> | | | - |
| k3 | <span class="supported"></span> | | | - |
| k4 | <span class="supported"></span> | | | - |
{:.rwd-table}
{:.unsupported-features}

### \<feConvolveMatrix\> attributes
{:.unsupported-features}

| Attributes | SVG Full 1.1 | SVG Tiny 1.2 | AmanithSVG | Notes |
| ---------- | :----------: | :----------: | :--------: | ----- |
| [Core attributes](#core-attributes) | | | | - |
| [Presentation attributes](#presentation-attributes) | | | | - |
| [Filter primitive attributes](#filter-primitive-attributes) | | | | - |
| class | <span class="supported"></span> | | | - |
| style | <span class="supported"></span> | | | - |
| in | <span class="supported"></span> | | | - |
| order | <span class="supported"></span> | | | - |
| kernelMatrix | <span class="supported"></span> | | | - |
| divisor | <span class="supported"></span> | | | - |
| bias | <span class="supported"></span> | | | - |
| targetX | <span class="supported"></span> | | | - |
| targetY | <span class="supported"></span> | | | - |
| edgeMode | <span class="supported"></span> | | | - |
| kernelUnitLength | <span class="supported"></span> | | | - |
| preserveAlpha | <span class="supported"></span> | | | - |
{:.rwd-table}
{:.unsupported-features}

### \<feDiffuseLighting\> attributes
{:.unsupported-features}

| Attributes | SVG Full 1.1 | SVG Tiny 1.2 | AmanithSVG | Notes |
| ---------- | :----------: | :----------: | :--------: | ----- |
| [Core attributes](#core-attributes) | | | | - |
| [Presentation attributes](#presentation-attributes) | | | | - |
| [Filter primitive attributes](#filter-primitive-attributes) | | | | - |
| class | <span class="supported"></span> | | | - |
| style | <span class="supported"></span> | | | - |
| in | <span class="supported"></span> | | | - |
| surfaceScale | <span class="supported"></span> | | | - |
| diffuseConstant | <span class="supported"></span> | | | - |
| kernelUnitLength | <span class="supported"></span> | | | - |
{:.rwd-table}
{:.unsupported-features}

### \<feDisplacementMap\> attributes
{:.unsupported-features}

| Attributes | SVG Full 1.1 | SVG Tiny 1.2 | AmanithSVG | Notes |
| ---------- | :----------: | :----------: | :--------: | ----- |
| [Core attributes](#core-attributes) | | | | - |
| [Presentation attributes](#presentation-attributes) | | | | - |
| [Filter primitive attributes](#filter-primitive-attributes) | | | | - |
| class | <span class="supported"></span> | | | - |
| style | <span class="supported"></span> | | | - |
| in | <span class="supported"></span> | | | - |
| in2 | <span class="supported"></span> | | | - |
| scale | <span class="supported"></span> | | | - |
| xChannelSelector | <span class="supported"></span> | | | - |
| yChannelSelector | <span class="supported"></span> | | | - |
{:.rwd-table}
{:.unsupported-features}

### \<feFlood\> attributes
{:.unsupported-features}

| Attributes | SVG Full 1.1 | SVG Tiny 1.2 | AmanithSVG | Notes |
| ---------- | :----------: | :----------: | :--------: | ----- |
| [Core attributes](#core-attributes) | | | | - |
| [Presentation attributes](#presentation-attributes) | | | | - |
| [Filter primitive attributes](#filter-primitive-attributes) | | | | - |
| class | <span class="supported"></span> | | | - |
| style | <span class="supported"></span> | | | - |
| flood-color | <span class="supported"></span> | | | - |
| flood-opacity | <span class="supported"></span> | | | - |
{:.rwd-table}
{:.unsupported-features}

### \<feGaussianBlur\> attributes
{:.unsupported-features}

| Attributes | SVG Full 1.1 | SVG Tiny 1.2 | AmanithSVG | Notes |
| ---------- | :----------: | :----------: | :--------: | ----- |
| [Core attributes](#core-attributes) | | | | - |
| [Presentation attributes](#presentation-attributes) | | | | - |
| [Filter primitive attributes](#filter-primitive-attributes) | | | | - |
| class | <span class="supported"></span> | | | - |
| style | <span class="supported"></span> | | | - |
| in | <span class="supported"></span> | | | - |
| stdDeviation | <span class="supported"></span> | | | - |
{:.rwd-table}
{:.unsupported-features}

### \<feImage\> attributes
{:.unsupported-features}

| Attributes | SVG Full 1.1 | SVG Tiny 1.2 | AmanithSVG | Notes |
| ---------- | :----------: | :----------: | :--------: | ----- |
| [Core attributes](#core-attributes) | | | | - |
| [Presentation attributes](#presentation-attributes) | | | | - |
| [Filter primitive attributes](#filter-primitive-attributes) | | | | - |
| [Xlink attributes](#xlink-attributes) | | | | - |
| class | <span class="supported"></span> | | | - |
| style | <span class="supported"></span> | | | - |
| externalResourcesRequired | <span class="supported"></span> | | | - |
| preserveAspectRatio | <span class="supported"></span> | | | - |
| xlink:href | <span class="supported"></span> | | | - |
{:.rwd-table}
{:.unsupported-features}

### \<feMerge\> attributes
{:.unsupported-features}

| Attributes | SVG Full 1.1 | SVG Tiny 1.2 | AmanithSVG | Notes |
| ---------- | :----------: | :----------: | :--------: | ----- |
| [Core attributes](#core-attributes) | | | | - |
| [Presentation attributes](#presentation-attributes) | | | | - |
| [Filter primitive attributes](#filter-primitive-attributes) | | | | - |
| class | <span class="supported"></span> | | | - |
| style | <span class="supported"></span> | | | - |
{:.rwd-table}
{:.unsupported-features}

### \<feMergeNode\> attributes
{:.unsupported-features}

| Attributes | SVG Full 1.1 | SVG Tiny 1.2 | AmanithSVG | Notes |
| ---------- | :----------: | :----------: | :--------: | ----- |
| [Core attributes](#core-attributes) | | | | - |
| in | <span class="supported"></span> | | | - |
{:.rwd-table}
{:.unsupported-features}

### \<feMorphology\> attributes
{:.unsupported-features}

| Attributes | SVG Full 1.1 | SVG Tiny 1.2 | AmanithSVG | Notes |
| ---------- | :----------: | :----------: | :--------: | ----- |
| [Core attributes](#core-attributes) | | | | - |
| [Presentation attributes](#presentation-attributes) | | | | - |
| [Filter primitive attributes](#filter-primitive-attributes) | | | | - |
| class | <span class="supported"></span> | | | - |
| style | <span class="supported"></span> | | | - |
| in | <span class="supported"></span> | | | - |
| operator | <span class="supported"></span> | | | - |
| radius | <span class="supported"></span> | | | - |
{:.rwd-table}
{:.unsupported-features}

### \<feOffset\> attributes
{:.unsupported-features}

| Attributes | SVG Full 1.1 | SVG Tiny 1.2 | AmanithSVG | Notes |
| ---------- | :----------: | :----------: | :--------: | ----- |
| [Core attributes](#core-attributes) | | | | - |
| [Presentation attributes](#presentation-attributes) | | | | - |
| [Filter primitive attributes](#filter-primitive-attributes) | | | | - |
| class | <span class="supported"></span> | | | - |
| style | <span class="supported"></span> | | | - |
| in | <span class="supported"></span> | | | - |
| dx | <span class="supported"></span> | | | - |
| dy | <span class="supported"></span> | | | - |
{:.rwd-table}
{:.unsupported-features}

### \<feSpecularLighting\> attributes
{:.unsupported-features}

| Attributes | SVG Full 1.1 | SVG Tiny 1.2 | AmanithSVG | Notes |
| ---------- | :----------: | :----------: | :--------: | ----- |
| [Core attributes](#core-attributes) | | | | - |
| [Presentation attributes](#presentation-attributes) | | | | - |
| [Filter primitive attributes](#filter-primitive-attributes) | | | | - |
| class | <span class="supported"></span> | | | - |
| style | <span class="supported"></span> | | | - |
| in | <span class="supported"></span> | | | - |
| surfaceScale | <span class="supported"></span> | | | - |
| specularConstant | <span class="supported"></span> | | | - |
| specularExponent | <span class="supported"></span> | | | - |
| kernelUnitLength | <span class="supported"></span> | | | - |
{:.rwd-table}
{:.unsupported-features}

### \<feTile\> attributes
{:.unsupported-features}

| Attributes | SVG Full 1.1 | SVG Tiny 1.2 | AmanithSVG | Notes |
| ---------- | :----------: | :----------: | :--------: | ----- |
| [Core attributes](#core-attributes) | | | | - |
| [Presentation attributes](#presentation-attributes) | | | | - |
| [Filter primitive attributes](#filter-primitive-attributes) | | | | - |
| class | <span class="supported"></span> | | | - |
| style | <span class="supported"></span> | | | - |
| in | <span class="supported"></span> | | | - |
{:.rwd-table}
{:.unsupported-features}

### \<feTurbulence\> attributes
{:.unsupported-features}

| Attributes | SVG Full 1.1 | SVG Tiny 1.2 | AmanithSVG | Notes |
| ---------- | :----------: | :----------: | :--------: | ----- |
| [Core attributes](#core-attributes) | | | | - |
| [Presentation attributes](#presentation-attributes) | | | | - |
| [Filter primitive attributes](#filter-primitive-attributes) | | | | - |
| class | <span class="supported"></span> | | | - |
| style | <span class="supported"></span> | | | - |
| baseFrequency | <span class="supported"></span> | | | - |
| numOctaves | <span class="supported"></span> | | | - |
| seed | <span class="supported"></span> | | | - |
| stitchTiles | <span class="supported"></span> | | | - |
| type | <span class="supported"></span> | | | - |
{:.rwd-table}
{:.unsupported-features}

---


## Interactivity elements

### \<cursor\> attributes
{:.unsupported-features}

| Attributes | SVG Full 1.1 | SVG Tiny 1.2 | AmanithSVG | Notes |
| ---------- | :----------: | :----------: | :--------: | ----- |
| [Conditional processing attributes](#conditional-processing-attributes) | | | | - |
| [Core attributes](#core-attributes) | | | | - |
| [Xlink attributes](#xlink-attributes) | | | | - |
| externalResourcesRequired | <span class="supported"></span> | | | - |
| x | <span class="supported"></span> | | | - |
| y | <span class="supported"></span> | | | - |
| xlink:href | <span class="supported"></span> | | | - |
{:.rwd-table}
{:.unsupported-features}

---


## Linking elements

### \<a\> attributes
{:.unsupported-features}

| Attributes | SVG Full 1.1 | SVG Tiny 1.2 | AmanithSVG | Notes |
| ---------- | :----------: | :----------: | :--------: | ----- |
| [Conditional processing attributes](#conditional-processing-attributes) | | | | - |
| [Core attributes](#core-attributes) | | | | - |
| [Graphical event attributes](#graphical-event-attributes) | | | | - |
| [Presentation attributes](#presentation-attributes) | | | | - |
| [Xlink attributes](#xlink-attributes) | | | | - |
| class | <span class="supported"></span> | <span class="unsupported"></span> | | - |
| style | <span class="supported"></span> | <span class="unsupported"></span> | | - |
| externalResourcesRequired | <span class="supported"></span> | <span class="unsupported"></span> | | - |
| transform | <span class="supported"></span> | <span class="supported"></span> | | - |
| xlink:href | <span class="supported"></span> | <span class="supported"></span> | | - |
| xlink:show | <span class="supported"></span> | <span class="supported"></span> | | - |
| xlink:actuate | <span class="supported"></span> | <span class="supported"></span> | | - |
| target | <span class="supported"></span> | <span class="supported"></span> | | - |
{:.rwd-table}
{:.unsupported-features}

### \<view\> attributes
{:.unsupported-features}

| Attributes | SVG Full 1.1 | SVG Tiny 1.2 | AmanithSVG | Notes |
| ---------- | :----------: | :----------: | :--------: | ----- |
| [Core attributes](#core-attributes) | | | | - |
| externalResourcesRequired | <span class="supported"></span> | <span class="supported"></span> | | - |
| viewBox | <span class="supported"></span> | <span class="supported"></span> | | - |
| preserveAspectRatio | <span class="supported"></span> | <span class="supported"></span> | | - |
| zoomAndPan | <span class="supported"></span> | <span class="supported"></span> | | - |
| viewTarget | <span class="supported"></span> | <span class="supported"></span> | | - |
{:.rwd-table}
{:.unsupported-features}

---


## Scripting elements

### \<script\> attributes
{:.unsupported-features}

| Attributes | SVG Full 1.1 | SVG Tiny 1.2 | AmanithSVG | Notes |
| ---------- | :----------: | :----------: | :--------: | ----- |
| [Core attributes](#core-attributes) | | | | - |
| [Xlink attributes](#xlink-attributes) | | | | - |
| externalResourcesRequired | <span class="supported"></span> | <span class="unsupported"></span> | | - |
| type | <span class="supported"></span> | <span class="supported"></span> | | - |
| xlink:href | <span class="supported"></span> | <span class="supported"></span> | | - |
{:.rwd-table}
{:.unsupported-features}

---


## Animation elements

### \<animate\> attributes
{:.unsupported-features}

| Attributes | SVG Full 1.1 | SVG Tiny 1.2 | AmanithSVG | Notes |
| ---------- | :----------: | :----------: | :--------: | ----- |
| [Conditional processing attributes](#conditional-processing-attributes) | | | | - |
| [Core attributes](#core-attributes) | | | | - |
| [Animation event attributes](#animation-event-attributes) | | | | - |
| [Xlink attributes](#xlink-attributes) | | | | - |
| [Animation attribute target attributes](#animation-attribute-target-attributes) | | | | - |
| [Animation timing attributes](#animation-timing-attributes) | | | | - |
| [Animation value attributes](#animation-value-attributes) | | | | - |
| [Animation addition attributes](#animation-addition-attributes) | | | | - |
| [Presentation attributes](#presentation-attributes) | | | | - |
| externalResourcesRequired | <span class="supported"></span> | <span class="unsupported"></span> | | - |
{:.rwd-table}
{:.unsupported-features}

### \<set\> attributes
{:.unsupported-features}

| Attributes | SVG Full 1.1 | SVG Tiny 1.2 | AmanithSVG | Notes |
| ---------- | :----------: | :----------: | :--------: | ----- |
| [Conditional processing attributes](#conditional-processing-attributes) | | | | - |
| [Core attributes](#core-attributes) | | | | - |
| [Animation event attributes](#animation-event-attributes) | | | | - |
| [Xlink attributes](#xlink-attributes) | | | | - |
| [Animation attribute target attributes](#animation-attribute-target-attributes) | | | | - |
| [Animation timing attributes](#animation-timing-attributes) | | | | - |
| externalResourcesRequired | <span class="supported"></span> | <span class="unsupported"></span> | | - |
| to | <span class="supported"></span> | <span class="supported"></span> | | - |
{:.rwd-table}
{:.unsupported-features}

### \<animateMotion\> attributes
{:.unsupported-features}

| Attributes | SVG Full 1.1 | SVG Tiny 1.2 | AmanithSVG | Notes |
| ---------- | :----------: | :----------: | :--------: | ----- |
| [Conditional processing attributes](#conditional-processing-attributes) | | | | - |
| [Core attributes](#core-attributes) | | | | - |
| [Animation event attributes](#animation-event-attributes) | | | | - |
| [Xlink attributes](#xlink-attributes) | | | | - |
| [Animation timing attributes](#animation-timing-attributes) | | | | - |
| [Animation value attributes](#animation-value-attributes) | | | | - |
| [Animation addition attributes](#animation-addition-attributes) | | | | - |
| externalResourcesRequired | <span class="supported"></span> | <span class="unsupported"></span> | | - |
| path | <span class="supported"></span> | <span class="supported"></span> | | - |
| keyPoints | <span class="supported"></span> | <span class="supported"></span> | | - |
| rotate | <span class="supported"></span> | <span class="supported"></span> | | - |
| origin | <span class="supported"></span> | <span class="supported"></span> | | - |
{:.rwd-table}
{:.unsupported-features}

### \<animateColor\> attributes
{:.unsupported-features}

| Attributes | SVG Full 1.1 | SVG Tiny 1.2 | AmanithSVG | Notes |
| ---------- | :----------: | :----------: | :--------: | ----- |
| [Conditional processing attributes](#conditional-processing-attributes) | | | | - |
| [Core attributes](#core-attributes) | | | | - |
| [Animation event attributes](#animation-event-attributes) | | | | - |
| [Xlink attributes](#xlink-attributes) | | | | - |
| [Animation attribute target attributes](#animation-attribute-target-attributes) | | | | - |
| [Animation timing attributes](#animation-timing-attributes) | | | | - |
| [Animation value attributes](#animation-value-attributes) | | | | - |
| [Animation addition attributes](#animation-addition-attributes) | | | | - |
| [Presentation attributes](#presentation-attributes) | | | | - |
| externalResourcesRequired | <span class="supported"></span> | <span class="unsupported"></span> | | - |
{:.rwd-table}
{:.unsupported-features}

### \<animateTransform\> attributes
{:.unsupported-features}

| Attributes | SVG Full 1.1 | SVG Tiny 1.2 | AmanithSVG | Notes |
| ---------- | :----------: | :----------: | :--------: | ----- |
| [Conditional processing attributes](#conditional-processing-attributes) | | | | - |
| [Core attributes](#core-attributes) | | | | - |
| [Animation event attributes](#animation-event-attributes) | | | | - |
| [Xlink attributes](#xlink-attributes) | | | | - |
| [Animation attribute target attributes](#animation-attribute-target-attributes) | | | | - |
| [Animation timing attributes](#animation-timing-attributes) | | | | - |
| [Animation value attributes](#animation-value-attributes) | | | | - |
| [Animation addition attributes](#animation-addition-attributes) | | | | - |
| externalResourcesRequired | <span class="supported"></span> | <span class="unsupported"></span> | | - |
| type | <span class="supported"></span> | <span class="supported"></span> | | - |
{:.rwd-table}
{:.unsupported-features}

### \<mpath\> attributes
{:.unsupported-features}

| Attributes | SVG Full 1.1 | SVG Tiny 1.2 | AmanithSVG | Notes |
| ---------- | :----------: | :----------: | :--------: | ----- |
| [Core attributes](#core-attributes) | | | | - |
| [Xlink attributes](#xlink-attributes) | | | | - |
| externalResourcesRequired | <span class="supported"></span> | <span class="unsupported"></span> | | - |
| xlink:href | <span class="supported"></span> | <span class="supported"></span> | | - |
{:.rwd-table}
{:.unsupported-features}

---


## Fonts elements

### \<font\> attributes
{:.unsupported-features}

| Attributes | SVG Full 1.1 | SVG Tiny 1.2 | AmanithSVG | Notes |
| ---------- | :----------: | :----------: | :--------: | ----- |
| [Core attributes](#core-attributes) | | | | - |
| [Presentation attributes](#presentation-attributes) | | | | - |
|  class | <span class="supported"></span> | <span class="unsupported"></span> | | - |
|  style | <span class="supported"></span> | <span class="unsupported"></span> | | - |
|  externalResourcesRequired | <span class="supported"></span> | <span class="unsupported"></span> | | - |
|  horiz-origin-x | <span class="supported"></span> | <span class="supported"></span> | | - |
|  horiz-origin-y | <span class="supported"></span> | <span class="unsupported"></span> | | - |
|  horiz-adv-x | <span class="supported"></span> | <span class="supported"></span> | | - |
|  vert-origin-x | <span class="supported"></span> | <span class="unsupported"></span> | | - |
|  vert-origin-y | <span class="supported"></span> | <span class="unsupported"></span> | | - |
|  vert-adv-y | <span class="supported"></span> | <span class="unsupported"></span> | | - |
{:.rwd-table}
{:.unsupported-features}

### \<glyph\> attributes
{:.unsupported-features}

| Attributes | SVG Full 1.1 | SVG Tiny 1.2 | AmanithSVG | Notes |
| ---------- | :----------: | :----------: | :--------: | ----- |
| [Core attributes](#core-attributes) | | | | - |
| [Presentation attributes](#presentation-attributes) | | | | - |
| class | <span class="supported"></span> | <span class="unsupported"></span> | | - |
| style | <span class="supported"></span> | <span class="unsupported"></span> | | - |
| d | <span class="supported"></span> | <span class="supported"></span> | | - |
| horiz-adv-x | <span class="supported"></span> | <span class="supported"></span> | | - |
| vert-origin-x | <span class="supported"></span> | <span class="unsupported"></span> | | - |
| vert-origin-y | <span class="supported"></span> | <span class="unsupported"></span> | | - |
| vert-adv-y | <span class="supported"></span> | <span class="unsupported"></span> | | - |
| unicode | <span class="supported"></span> | <span class="supported"></span> | | - |
| glyph-name | <span class="supported"></span> | <span class="supported"></span> | | - |
| orientation | <span class="supported"></span> | <span class="unsupported"></span> | | - |
| arabic-form | <span class="supported"></span> | <span class="supported"></span> | | - |
| lang | <span class="supported"></span> | <span class="supported"></span> | | - |
{:.rwd-table}
{:.unsupported-features}

### \<missing-glyph\> attributes
{:.unsupported-features}

| Attributes | SVG Full 1.1 | SVG Tiny 1.2 | AmanithSVG | Notes |
| ---------- | :----------: | :----------: | :--------: | ----- |
| [Core attributes](#core-attributes) | | | | - |
| [Presentation attributes](#presentation-attributes) | | | | - |
| class | <span class="supported"></span> | <span class="unsupported"></span> | | - |
| style | <span class="supported"></span> | <span class="unsupported"></span> | | - |
| d | <span class="supported"></span> | <span class="supported"></span> | | - |
| horiz-adv-x | <span class="supported"></span> | <span class="supported"></span> | | - |
| vert-origin-x | <span class="supported"></span> | <span class="unsupported"></span> | | - |
| vert-origin-y | <span class="supported"></span> | <span class="unsupported"></span> | | - |
| vert-adv-y | <span class="supported"></span> | <span class="unsupported"></span> | | - |
{:.rwd-table}
{:.unsupported-features}

### \<hkern\> attributes
{:.unsupported-features}

| Attributes | SVG Full 1.1 | SVG Tiny 1.2 | AmanithSVG | Notes |
| ---------- | :----------: | :----------: | :--------: | ----- |
| [Core attributes](#core-attributes) | | | | - |
| u1 | <span class="supported"></span> | <span class="supported"></span> | | - |
| g1 | <span class="supported"></span> | <span class="supported"></span> | | - |
| u2 | <span class="supported"></span> | <span class="supported"></span> | | - |
| g2 | <span class="supported"></span> | <span class="supported"></span> | | - |
| k | <span class="supported"></span> | <span class="supported"></span> | | - |
{:.rwd-table}
{:.unsupported-features}

### \<vkern\> attributes
{:.unsupported-features}

| Attributes | SVG Full 1.1 | SVG Tiny 1.2 | AmanithSVG | Notes |
| ---------- | :----------: | :----------: | :--------: | ----- |
| [Core attributes](#core-attributes) | | | | - |
| u1 | <span class="supported"></span> | <span class="supported"></span> | | - |
| g1 | <span class="supported"></span> | <span class="supported"></span> | | - |
| u2 | <span class="supported"></span> | <span class="supported"></span> | | - |
| g2 | <span class="supported"></span> | <span class="supported"></span> | | - |
| k | <span class="supported"></span> | <span class="supported"></span> | | - |
{:.rwd-table}
{:.unsupported-features}

### \<font-face\> attributes
{:.unsupported-features}

| Attributes | SVG Full 1.1 | SVG Tiny 1.2 | AmanithSVG | Notes |
| ---------- | :----------: | :----------: | :--------: | ----- |
| [Core attributes](#core-attributes) | | | | - |
| font-family | <span class="supported"></span> | <span class="supported"></span> | | - |
| font-style | <span class="supported"></span> | <span class="supported"></span> | | - |
| font-variant | <span class="supported"></span> | <span class="unsupported"></span> | | - |
| font-weight | <span class="supported"></span> | <span class="supported"></span> | | - |
| font-stretch | <span class="supported"></span> | <span class="unsupported"></span> | | - |
| font-size | <span class="supported"></span> | <span class="unsupported"></span> | | - |
| unicode-range | <span class="supported"></span> | <span class="supported"></span> | | - |
| units-per-em | <span class="supported"></span> | <span class="supported"></span> | | - |
| panose-1 | <span class="supported"></span> | <span class="supported"></span> | | - |
| stemv | <span class="supported"></span> | <span class="supported"></span> | | - |
| stemh | <span class="supported"></span> | <span class="supported"></span> | | - |
| slope | <span class="supported"></span> | <span class="supported"></span> | | - |
| cap-height | <span class="supported"></span> | <span class="supported"></span> | | - |
| x-height | <span class="supported"></span> | <span class="supported"></span> | | - |
| accent-height | <span class="supported"></span> | <span class="supported"></span> | | - |
| ascent | <span class="supported"></span> | <span class="supported"></span> | | - |
| descent | <span class="supported"></span> | <span class="supported"></span> | | - |
| widths | <span class="supported"></span> | <span class="supported"></span> | | - |
| bbox | <span class="supported"></span> | <span class="supported"></span> | | - |
| ideographic | <span class="supported"></span> | <span class="supported"></span> | | - |
| alphabetic | <span class="supported"></span> | <span class="supported"></span> | | - |
| mathematical | <span class="supported"></span> | <span class="supported"></span> | | - |
| hanging | <span class="supported"></span> | <span class="supported"></span> | | - |
| v-ideographic | <span class="supported"></span> | <span class="unsupported"></span> | | - |
| v-alphabetic | <span class="supported"></span> | <span class="unsupported"></span> | | - |
| v-mathematical | <span class="supported"></span> | <span class="unsupported"></span> | | - |
| v-hanging | <span class="supported"></span> | <span class="unsupported"></span> | | - |
| underline-position | <span class="supported"></span> | <span class="supported"></span> | | - |
| underline-thickness | <span class="supported"></span> | <span class="supported"></span> | | - |
| strikethrough-position | <span class="supported"></span> | <span class="supported"></span> | | - |
| strikethrough-thickness | <span class="supported"></span> | <span class="supported"></span> | | - |
| overline-position | <span class="supported"></span> | <span class="supported"></span> | | - |
| overline-thickness | <span class="supported"></span> | <span class="supported"></span> | | - |
{:.rwd-table}
{:.unsupported-features}

### \<font-face-src\> attributes
{:.unsupported-features}

| Attributes | SVG Full 1.1 | SVG Tiny 1.2 | AmanithSVG | Notes |
| ---------- | :----------: | :----------: | :--------: | ----- |
| [Core attributes](#core-attributes) | | | | - |
{:.rwd-table}
{:.unsupported-features}

### \<font-face-uri\> attributes
{:.unsupported-features}

| Attributes | SVG Full 1.1 | SVG Tiny 1.2 | AmanithSVG | Notes |
| ---------- | :----------: | :----------: | :--------: | ----- |
| [Core attributes](#core-attributes) | | | | - |
| [Xlink attributes](#xlink-attributes) | | | | - |
| xlink:href | <span class="supported"></span> | <span class="supported"></span> | | - |
{:.rwd-table}
{:.unsupported-features}

### \<font-face-format\> attributes
{:.unsupported-features}

| Attributes | SVG Full 1.1 | SVG Tiny 1.2 | AmanithSVG | Notes |
| ---------- | :----------: | :----------: | :--------: | ----- |
| [Core attributes](#core-attributes) | | | | - |
| string | <span class="supported"></span> | | | - |
{:.rwd-table}
{:.unsupported-features}

### \<font-face-name\> attributes
{:.unsupported-features}

| Attributes | SVG Full 1.1 | SVG Tiny 1.2 | AmanithSVG | Notes |
| ---------- | :----------: | :----------: | :--------: | ----- |
| [Core attributes](#core-attributes) | | | | - |
| name | <span class="supported"></span> | | | - |
{:.rwd-table}
{:.unsupported-features}

---


## Metadate elements

### \<metadata\> attributes
{:.unsupported-features}

| Attributes | SVG Full 1.1 | SVG Tiny 1.2 | AmanithSVG | Notes |
| ---------- | :----------: | :----------: | :--------: | ----- |
| [Core attributes](#core-attributes) | | | | - |
{:.rwd-table}
{:.unsupported-features}

---


## Extensibility elements

### \<foreignObject\> attributes
{:.unsupported-features}

| Attributes | SVG Full 1.1 | SVG Tiny 1.2 | AmanithSVG | Notes |
| ---------- | :----------: | :----------: | :--------: | ----- |
| [Conditional processing attributes](#conditional-processing-attributes) | | | | - |
| [Core attributes](#core-attributes) | | | | - |
| [Graphical event attributes](#graphical-event-attributes) | | | | - |
| [Presentation attributes](#presentation-attributes) | | | | - |
| class | <span class="supported"></span> | <span class="unsupported"></span> | | - |
| style | <span class="supported"></span> | <span class="unsupported"></span> | | - |
| externalResourcesRequired | <span class="supported"></span> | <span class="unsupported"></span> | | - |
| transform | <span class="supported"></span> | <span class="supported"></span> | | - |
| x | <span class="supported"></span> | <span class="supported"></span> | | - |
| y | <span class="supported"></span> | <span class="supported"></span> | | - |
| width | <span class="supported"></span> | <span class="supported"></span> | | - |
| height | <span class="supported"></span> | <span class="supported"></span> | | - |
{:.rwd-table}
{:.unsupported-features}



---
