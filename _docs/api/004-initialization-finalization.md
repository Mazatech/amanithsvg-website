---
layout: default
title: "Initialization and finalization"
date: 2018-01-01 08:00:00 +0100
chapter: 4
categories: [api]
---

# Initialization and finalization

## Initialization

Before to use any functionality, AmanithSVG must be initialized by calling `svgtInit` function:

```c
/*
    Initialize the library.
    This function does not set the last-error code (see svgtGetLastError).

    Return SVGT_NO_ERROR if the operation was completed successfully, else
    an error code.
    NB: in multi-thread applications, this function must be called once, before
    any created/spawned thread makes use of the library functions.
*/
SVGTErrorCode svgtInit(SVGTuint screenWidth,
                       SVGTuint screenHeight,
                       SVGTfloat dpi);
```

The function takes as input the information relative to the device screen: width/height (in pixels) and dpi (dots per inch). Such information are used to resolve SVG lengths when expressed in percentage; the best way to obtain screen information is to use native calls:

### Windows

```c
screenWidth = GetSystemMetrics(SM_CXSCREEN);
screenHeight = GetSystemMetrics(SM_CYSCREEN);
dpi = GetDeviceCaps(hDC, LOGPIXELSX);
```

###  MacOS X

```objc
NSRect r;
NSScreen* screen = [NSScreen mainScreen];
NSDictionary* description = [screen deviceDescription];
NSSize displayPixelSize = [[description objectForKey:NSDeviceSize] sizeValue];
CGSize displayPhysicalSize = CGDisplayScreenSize([[description objectForKey:@"NSScreenNumber"] unsignedIntValue]);

r.origin.x = 0;
r.origin.y = 0;
r.size.width = displayPixelSize.width;
r.size.height = displayPixelSize.height;
// take care of Retina display
r = [screen convertRectToBacking :r];
screenWidth = r.size.width;
screenHeight = r.size.height;
// 25.4 millimetres in an inch
dpi = ((r.size.width / displayPhysicalSize.width) * 25.4f)
```

### Linux X11

```c
Display* display = XOpenDisplay(NULL);
int screen = DefaultScreen(display);
screenWidth = DisplayWidth(display, screen);
screenHeight = DisplayHeight(display, screen);
// 25.4 millimetres in an inch
dpi = ((screenWidth / DisplayWidthMM(display, screen)) * 25.4f);
```

### Android

```c
DisplayMetrics metrics = view.getResources().getDisplayMetrics();
screenWidth = metrics.widthPixels;
screenHeight = metrics.heightPixels;
dpi = metrics.xdpi;
```

### iOS

```objc
CGRect screenRect = [[UIScreen mainScreen] nativeBounds];
screenWidth = screenRect.size.width;
screenHeight = screenRect.size.height;
// calculate dpi according to the actual device
struct utsname systemInfo;
uname(&systemInfo);
NSString* code = [NSString stringWithCString:systemInfo.machine encoding:NSUTF8StringEncoding];
NSDictionary* deviceNamesByCode = @{
    // 1st Gen
    @"iPhone1,1"  :@163,
    // 3G
    @"iPhone1,2"  :@163,
    // 3GS
    @"iPhone2,1"  :@163,
    // 4
    @"iPhone3,1"  :@326,
    @"iPhone3,2"  :@326,
    @"iPhone3,3"  :@326,
    // 4S
    @"iPhone4,1"  :@326,
    // 5
    @"iPhone5,1"  :@326,
    @"iPhone5,2"  :@326,
    // 5c
    @"iPhone5,3"  :@326,
    @"iPhone5,4"  :@326,
    // 5s
    @"iPhone6,1"  :@326,
    @"iPhone6,2"  :@326,
    // 6 Plus
    @"iPhone7,1"  :@401,
    // 6
    @"iPhone7,2"  :@326,
    // 6s
    @"iPhone8,1"  :@326,
    // 6s Plus
    @"iPhone8,2"  :@401,
    // SE
    @"iPhone8,4"  :@326,
    // 7
    @"iPhone9,1"  :@326,
    @"iPhone9,3"  :@326,
    // 7 Plus
    @"iPhone9,2"  :@401,
    @"iPhone9,4"  :@401,
    // 8
    @"iPhone10,1" :@326,
    @"iPhone10,4" :@326,
    // 8 Plus
    @"iPhone10,2" :@401,
    @"iPhone10,5" :@401,
    // X
    @"iPhone10,3" :@458,
    @"iPhone10,6" :@458,
    // XR
    @"iPhone11,8" :@326,
    // XS
    @"iPhone11,2" :@458,
    // XS Max
    @"iPhone11,4" :@458,
    @"iPhone11,6" :@458,
    // 11
    @"iPhone12,1" :@326,
    // 11 Pro
    @"iPhone12,3" :@458,
    // 11 Pro Max
    @"iPhone12,5" :@458,
    // SE 2
    @"iPhone12,8" :@326,
    // 12 mini
    @"iPhone13,1" :@476,
    // 12
    @"iPhone13,2" :@460,
    // 12 Pro
    @"iPhone13,3" :@460,
    // 12 Pro Max
    @"iPhone13,4" :@458,
    // 13 mini
    @"iPhone14,4" :@476,
    // 13
    @"iPhone14,5" :@460,
    // 13 Pro
    @"iPhone14,2" :@460,
    // 13 Pro Max
    @"iPhone14,3" :@458,
    // SE 3rd Gen
    @"iPhone14,6" :@326,
    // 14
    @"iPhone14,7" :@460,
    // 14 Plus
    @"iPhone14,8" :@458,
    // 14 Pro
    @"iPhone15,2" :@460,
    // 14 Pro Max
    @"iPhone15,3" :@460,
    // 1
    @"iPad1,1"    :@132,
    // 2
    @"iPad2,1"    :@132,
    @"iPad2,2"    :@132,
    @"iPad2,3"    :@132,
    @"iPad2,4"    :@132,
    // Mini
    @"iPad2,5"    :@163,
    @"iPad2,6"    :@163,
    @"iPad2,7"    :@163,
    // 3
    @"iPad3,1"    :@264,
    @"iPad3,2"    :@264,
    @"iPad3,3"    :@264,
    // 4
    @"iPad3,4"    :@264,
    @"iPad3,5"    :@264,
    @"iPad3,6"    :@264,
    // Air
    @"iPad4,1"    :@264,
    @"iPad4,2"    :@264,
    @"iPad4,3"    :@264,
    // Mini 2
    @"iPad4,4"    :@326,
    @"iPad4,5"    :@326,
    @"iPad4,6"    :@326,
    // Mini 3
    @"iPad4,7"    :@326,
    @"iPad4,8"    :@326,
    @"iPad4,9"    :@326,
    // Mini 4
    @"iPad5,1"    :@326,
    @"iPad5,2"    :@326,
    // Air 2
    @"iPad5,3"    :@264,
    @"iPad5,4"    :@264,
    // Pro 12.9-inch
    @"iPad6,7"    :@264,
    @"iPad6,8"    :@264,
    // Pro 9.7-inch
    @"iPad6,3"    :@264,
    @"iPad6,4"    :@264,
    // iPad 5th Gen, 2017
    @"iPad6,11"   :@264,
    @"iPad6,12"   :@264,
    // Pro 12.9-inch, 2017
    @"iPad7,1"    :@264,
    @"iPad7,2"    :@264,
    // Pro 10.5-inch, 2017
    @"iPad7,3"    :@264,
    @"iPad7,4"    :@264,
    // iPad 6th Gen, 2018
    @"iPad7,5"    :@264,
    @"iPad7,6"    :@264,
    // iPad Pro 3rd Gen 11-inch, 2018
    @"iPad8,1"    :@264,
    @"iPad8,3"    :@264,
    // iPad Pro 3rd Gen 11-inch 1TB, 2018
    @"iPad8,2"    :@264,
    @"iPad8,4"    :@264,
    // iPad Pro 3rd Gen 12.9-inch, 2018
    @"iPad8,5"    :@264,
    @"iPad8,7"    :@264,
    // iPad Pro 3rd Gen 12.9-inch 1TB, 2018
    @"iPad8,6"    :@264,
    @"iPad8,8"    :@264,
    // iPad Pro 4rd Gen 11-inch, 2020
    @"iPad8,9"    :@264,
    @"iPad8,10"   :@264,
    // iPad Pro 4rd Gen 12.9-inch, 2020
    @"iPad8,11"   :@264,
    @"iPad8,12"   :@264,
    // Mini 5
    @"iPad11,1"   :@326,
    @"iPad11,2"   :@326,
    // Air 3
    @"iPad11,3"   :@264,
    @"iPad11,4"   :@264,
    // iPad 8th Gen, 2020
    @"iPad11,6"   :@264,
    @"iPad11,7"   :@264,
    // Air 4
    @"iPad13,1"   :@264,
    @"iPad13,2"   :@264,
    // iPad Pro 3rd Gen 11-inch, 2021
    @"iPad13,4"   :@264,
    @"iPad13,5"   :@264,
    @"iPad13,6"   :@264,
    @"iPad13,7"   :@264,
    // iPad Pro 5th Gen 12.9-inch, 2021
    @"iPad13,8"   :@264,
    @"iPad13,9"   :@264,
    @"iPad13,10"  :@264,
    @"iPad13,11"  :@264,
    // Air 5, 2022
    @"iPad13,16"  :@264,
    @"iPad13,17"  :@264,
    // iPad Pro 3rd Gen 11-inch, 2021
    @"iPad13,4"   :@264,
    @"iPad13,5"   :@264,
    @"iPad13,6"   :@264,
    @"iPad13,7"   :@264,
    // iPad Pro 5th Gen 12.9-inch, 2021
    @"iPad13,8"   :@264,
    @"iPad13,9"   :@264,
    @"iPad13,10"  :@264,
    @"iPad13,11"  :@264,
    // Air 5, 2022
    @"iPad13,16"  :@264,
    @"iPad13,17"  :@264,
    // mini 6
    @"iPad14,1"   :@326,
    @"iPad14,2"   :@326,
    // iPad 9th Gen, 2021
    @"iPad12,1"   :@264,
    @"iPad12,2"   :@264,
    // iPad 10th Gen, 2022
    @"iPad13,18"  :@264,
    @"iPad13,19"  :@264,
    // iPad Pro 4th Gen 11-inch, 2022
    @"iPad14,3"   :@264,
    @"iPad14,4"   :@264,
    // iPad Pro 6th Gen 12.9-inch, 2022
    @"iPad14,5"   :@264,
    @"iPad14,6"   :@264,
    // 1st Gen
    @"iPod1,1"    :@163,
    // 2nd Gen
    @"iPod2,1"    :@163,
    // 3rd Gen
    @"iPod3,1"    :@163,
    // 4th Gen
    @"iPod4,1"    :@326,
    // 5th Gen
    @"iPod5,1"    :@326,
    // 6th Gen
    @"iPod7,1"    :@326,
    // 7th Gen
    @"iPod9,1"    :@326
};

dpi = [[deviceNamesByCode objectForKey:code] integerValue];
if (!dpi) {
    SVGTfloat scale = 1.0f;
    if ([[UIScreen mainScreen] respondsToSelector:@selector(scale)]) {
        scale = [[UIScreen mainScreen] scale];
    }
    // dpi estimation (not accurate)
    if (UI_USER_INTERFACE_IDIOM() == UIUserInterfaceIdiomPad) {
        dpi = 132.0f * scale;
    }
    else
    if (UI_USER_INTERFACE_IDIOM() == UIUserInterfaceIdiomPhone) {
        dpi = 163.0f * scale;
    }
    else {
        dpi = 160.0f * scale;
    }
}
```

### libGDX

```java
screenWidth = Gdx.graphics.getBackBufferWidth();
screenHeight = Gdx.graphics.getBackBufferHeight();
dpi = Gdx.graphics.getPpiX();
```

NB: the `svgtInit` function returns a `SVGT_ILLEGAL_ARGUMENT_ERROR` error code if one of the passed parameters is less or equal to zero.

## Configuration

Before to start the creation of SVG documents, drawing surfaces and begin drawing operations, some configuration parameters can be passed to the library. In the detail, it is possible to:

- specify the curves quality: it forces the renderer to flatten the curves (i.e. generate polygon approximation) with more or less precision.
  `svgtConfigSet(SVGT_CONFIG_CURVES_QUALITY, <quality>)` where `quality` parameter ranges between `1` and `100`: `1` represents the worst quality and `100` the best one.
- specify the system/user-agent language; this setting will affect the conditional rendering of `<switch>` elements and elements with `systemLanguage` attribute specified.
  `svgtLanguageSet(<languages>)` where `languages` is list of languages separated by semicolon (e.g. `"en-US;en-GB;it;es"`).
- specify external resources; this setting allows the library to acknowledge about an external resource (font or image) along with possible hints; when needed, such resources will be used for the rendering of SVG documents.
  `svgtResourceSet(<id>, <buffer>, <bufferSize>, <type>, <hints>)`

```c
/*
    Configure parameters and thresholds for the AmanithSVG library.
    This function does not set the last-error code (see svgtGetLastError).

    This function can be called at any time, but it will have effect only if:

    - the library has not been already initialized by a previous call to
      svgtInit, or
    - the library has been already initialized (i.e. svgtInit has been already
      called) but no other functions, except for svgtMaxCurrentThreads,
      svgtSurfaceMaxDimension, svgtFontResourceSet, have been called.

    When 'config' refers to an integer parameter, the given 'value' is converted
    to an integer using a mathematical floor operation.

    This function returns:

    - SVGT_ILLEGAL_ARGUMENT_ERROR if the specified value is not valid for the
      given configuration parameter

    - SVGT_NO_ERROR if the library has been already initialized
      and one or more functions different than svgtMaxCurrentThreads,
      svgtSurfaceMaxDimension, svgtFontResourceSet, have been called.
      In this case the function does nothing: by calling svgtConfigGet, you
      will get back the same value that the configuration parameter had
      before the call to svgtConfigSet.

    - SVGT_NO_ERROR in all other cases (i.e. in case of success).
*/
SVGTErrorCode svgtConfigSet(SVGTConfig config,
                            SVGTfloat value);
```

Certain `SVGTConfig` values refer to read-only parameters (e.g. `SVGT_CONFIG_MAX_SURFACE_DIMENSION`). Calling `svgtConfigSet` on these parameters has no effect (`SVGT_NO_ERROR` is returned).

```c
/*
    Get the current value relative to the specified configuration parameter.
    This function does not set the last-error code (see svgtGetLastError).

    If the given parameter is invalid (i.e. it does not correspond to any
    value of the SVGTConfig enum type), a negative number is returned.
*/
SVGTfloat svgtConfigGet(SVGTConfig config);
```

Through `svgtConfigGet` it is also possible to get the value of some read-only parameters:

- `SVGT_CONFIG_MAX_CURRENT_THREADS` the maximum number of different threads that can "work" (e.g. create surfaces, draw documents, etc) concurrently
- `SVGT_CONFIG_MAX_SURFACE_DIMENSION` the maximum dimension allowed for drawing surfaces, in pixels


```c
/*
    Set the system / user-agent language.
    This function does not set the last-error code (see svgtGetLastError).

    The given argument must be a non-NULL, non-empty list of languages
    separated by semicolon (e.g. en-US;en-GB;it;es)

    This function can be called at any time, but it will have effect only if:

    - the library has not been already initialized by a previous call to
      svgtInit, or
    - the library has been already initialized (i.e. svgtInit has been already
      called) but no other functions, except for svgtMaxCurrentThreads,
      svgtSurfaceMaxDimension, svgtConfigGet, svgtConfigSet, svgtResourceSet
      have been called.

    This function returns:

    - SVGT_ILLEGAL_ARGUMENT_ERROR if the given string is NULL or empty

    - SVGT_NO_ERROR if the library has been already initialized
      and one or more functions different than svgtMaxCurrentThreads,
      svgtSurfaceMaxDimension, svgtResourceSet, have been called.
      In this case the function does nothing.

    - SVGT_NO_ERROR in all other cases (i.e. in case of success).

    NB: the library starts with a default "en" (generic English).
*/
SVGTErrorCode svgtLanguageSet(const char* languages);
```

```c
/*
    Get the maximum number of different threads that can "work" (e.g. create
    surfaces, create documents and draw them) concurrently.
    This function does not set the last-error code (see svgtGetLastError).

    In multi-thread applications, each thread can only draw documents it has
    created, on surfaces it has created. In other words, a thread cannot
    draw a document created by another thread or on surfaces belonging to
    other threads.

    The function can be called at any time and always returns a valid value.

    NB: this is a shortcut to svgtConfigGet(SVGT_CONFIG_MAX_CURRENT_THREADS)
    and may be removed in the future.
*/
SVGTuint svgtMaxCurrentThreads(void);
```

**WARNING** this function is a shortcut to `svgtConfigGet(SVGT_CONFIG_MAX_CURRENT_THREADS)` and may be removed in the future.

```c
/*
    Instruct the library about an external resource.
    All resources must be specified in advance after the call to svgtInit
    and before to create/spawn threads (i.e. no other functions, except
    for svgtMaxCurrentThreads, svgtSurfaceMaxDimension, svgtConfigGet,
    svgtConfigSet, svgtLanguageSet have been called).

    This function does not set the last-error code (see svgtGetLastError).

    'id' must be a not-empty null-terminated string.

    'buffer' must point to a read-only area containing the resource file.
    Such read-only buffer must be a valid and immutable memory area
    throughout the whole life of the application that uses the library.
    In multi-thread applications the 'buffer' memory must be accessible
    (readable) by all threads.

    'type' specifies the type of resource.

    'hints' must be a valid bitwise OR of values from the SVGTResourceHint
    enumeration.

    If a previous resource has been specified with the given 'id', the new
    provided 'buffer' pointer is used.

    This function returns:

    - SVGT_NOT_INITIALIZED_ERROR if the library has not yet been initialized
      (see svgtInit)

    - SVGT_ILLEGAL_ARGUMENT_ERROR if 'id' is NULL or an empty string

    - SVGT_ILLEGAL_ARGUMENT_ERROR if 'buffer' is NULL or if 'bufferSize' is zero

    - SVGT_ILLEGAL_ARGUMENT_ERROR if 'type' is not a valid value from the
      SVGTResourceType enumeration

    - SVGT_ILLEGAL_ARGUMENT_ERROR if 'hints' is not a bitwise OR of values from
      the SVGTResourceHint enumeration

    - SVGT_ILLEGAL_ARGUMENT_ERROR if 'type' is SVGT_RESOURCE_TYPE_IMAGE and
      'hints' is different than zero

    - SVGT_INVALID_RESOURCE_ERROR if 'type' is SVGT_RESOURCE_TYPE_FONT and the
      given buffer does not contain a valid font file

    - SVGT_INVALID_RESOURCE_ERROR if 'type' is SVGT_RESOURCE_TYPE_IMAGE and
      the given buffer does not contain a valid bitmap file

    - SVGT_NO_ERROR if the operation was completed successfully

    NB: vector fonts like OTF, TTF and WOFF are supported, bitmap fonts are not
    supported; as for images, only JPEG and PNG are supported (16 bits PNG are
    not supported).
*/
SVGTErrorCode svgtResourceSet(const char* id,
                              const SVGTubyte* buffer,
                              SVGTuint bufferSize,
                              SVGTResourceType type,
                              SVGTbitfield hints);
```

The types of supported resource are defined by the enumeration type `SVGTResourceType`.

| Resource type | Notes |
| ------ | -------------- |
| `SVGT_RESOURCE_TYPE_FONT` | Resource is a font (vector fonts like OTF, TTF and WOFF are supported, bitmap fonts are not supported) |
| `SVGT_RESOURCE_TYPE_IMAGE` | Resource is a bitmap/image (only JPEG and PNG are supported; 16 bits PNG are not supported) |
{:.rwd-table .rwd-table-resource-types}

SVG generic font families are a fallback mechanism, a means of preserving some of the SVG author's intent when none of the specified fonts are available. `svgtResourceSet` functions allows to tag some of the provided font resources as corresponding to one (or more) generic family name. The `hints` parameter is a bitwise `or` of values from the `SVGTResourceHint` enumeration:

| Resource hint | Notes |
| ------ | -------------- |
| `SVGT_RESOURCE_HINT_DEFAULT_SERIF_FONT` | Font resource must be selected when the `font-family` attribute matches the `serif` generic family |
| `SVGT_RESOURCE_HINT_DEFAULT_SANS_SERIF_FONT` | Font resource must be selected when the `font-family` attribute matches the `sans-serif` generic family |
| `SVGT_RESOURCE_HINT_DEFAULT_MONOSPACE_FONT` | Font resource must be selected when the `font-family` attribute matches the `monospace` generic family |
| `SVGT_RESOURCE_HINT_DEFAULT_CURSIVE_FONT` | Font resource must be selected when the `font-family` attribute matches the `cursive` generic family |
| `SVGT_RESOURCE_HINT_DEFAULT_FANTASY_FONT` | Font resource must be selected when the `font-family` attribute matches the `fantasy` generic family |
| `SVGT_RESOURCE_HINT_DEFAULT_SYSTEM_UI_FONT` | Font resource must be selected when the `font-family` attribute matches the `system-ui` generic family |
| `SVGT_RESOURCE_HINT_DEFAULT_UI_SERIF_FONT` | Font resource must be selected when the `font-family` attribute matches the `ui-serif` generic family |
| `SVGT_RESOURCE_HINT_DEFAULT_UI_SANS_SERIF_FONT` | Font resource must be selected when the `font-family` attribute matches the `ui-sans-serif` generic family |
| `SVGT_RESOURCE_HINT_DEFAULT_UI_MONOSPACE_FONT` | Font resource must be selected when the `font-family` attribute matches the `ui-monospace` generic family |
| `SVGT_RESOURCE_HINT_DEFAULT_UI_ROUNDED_FONT` | Font resource must be selected when the `font-family` attribute matches the `ui-rounded` generic family |
| `SVGT_RESOURCE_HINT_DEFAULT_EMOJI_FONT` | Font resource must be selected when the `font-family` attribute matches the `emoji` generic family |
| `SVGT_RESOURCE_HINT_DEFAULT_MATH_FONT` | Font resource must be selected when the `font-family` attribute matches the `math` generic family |
| `SVGT_RESOURCE_HINT_DEFAULT_FANGSONG_FONT` | Font resource must be selected when the `font-family` attribute matches the `fangsong` generic family |
{:.rwd-table .rwd-table-resource-hints}


## Finalization

Once finished to use AmanithSVG functionalities, it is advisable to call the `svgtDone` function, in order to be sure that all allocated SVG resources will be freed.

```c
/*
    Destroy the library, freeing all allocated resources.
    This function does not set the last-error code (see svgtGetLastError).

    In multi-thread applications, this function must be called once, when all
    created/spawned threads have finished using the library functions.
*/
void svgtDone(void);
```
---
