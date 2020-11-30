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
dpi = (screenWidth / DisplayWidthMM(display, screen)) * 25.4f);
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
    @"iPhone1,1" :@163,
    // 3G
    @"iPhone1,2" :@163,
    // 3GS
    @"iPhone2,1" :@163,
    // 4
    @"iPhone3,1" :@326,
    @"iPhone3,2" :@326,
    @"iPhone3,3" :@326,
    // 4S
    @"iPhone4,1" :@326,
    // 5
    @"iPhone5,1" :@326,
    @"iPhone5,2" :@326,
    // 5c
    @"iPhone5,3" :@326,
    @"iPhone5,4" :@326,
    // 5s
    @"iPhone6,1" :@326,
    @"iPhone6,2" :@326,
    // 6 Plus
    @"iPhone7,1" :@401,
    // 6
    @"iPhone7,2" :@326,
    // 6s
    @"iPhone8,1" :@326,
    // 6s Plus
    @"iPhone8,2" :@401,
    // SE
    @"iPhone8,4" :@326,
    // 7
    @"iPhone9,1" :@326,
    @"iPhone9,3" :@326,
    // 7 Plus
    @"iPhone9,2" :@401,
    @"iPhone9,4" :@401,
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
    // 1
    @"iPad1,1"   :@132,
    // 2
    @"iPad2,1"   :@132,
    @"iPad2,2"   :@132,
    @"iPad2,3"   :@132,
    @"iPad2,4"   :@132,
    // Mini
    @"iPad2,5"   :@163,
    @"iPad2,6"   :@163,
    @"iPad2,7"   :@163,
    // 3
    @"iPad3,1"   :@264,
    @"iPad3,2"   :@264,
    @"iPad3,3"   :@264,
    // 4
    @"iPad3,4"   :@264,
    @"iPad3,5"   :@264,
    @"iPad3,6"   :@264,
    // Air
    @"iPad4,1"   :@264,
    @"iPad4,2"   :@264,
    @"iPad4,3"   :@264,
    // Mini 2
    @"iPad4,4"   :@326,
    @"iPad4,5"   :@326,
    @"iPad4,6"   :@326,
    // Mini 3
    @"iPad4,7"   :@326,
    @"iPad4,8"   :@326,
    @"iPad4,9"   :@326,
    // Mini 4
    @"iPad5,1"   :@326,
    @"iPad5,2"   :@326,
    // Air 2
    @"iPad5,3"   :@264,
    @"iPad5,4"   :@264,
    // Pro 12.9-inch
    @"iPad6,7"   :@264,
    @"iPad6,8"   :@264,
    // Pro 9.7-inch
    @"iPad6,3"   :@264,
    @"iPad6,4"   :@264,
    // iPad 5th Gen, 2017
    @"iPad6,11"  :@264,
    @"iPad6,12"  :@264,
    // Pro 12.9-inch, 2017
    @"iPad7,1"   :@264,
    @"iPad7,2"   :@264,
    // Pro 10.5-inch, 2017
    @"iPad7,3"   :@264,
    @"iPad7,4"   :@264,
    // iPad 6th Gen, 2018
    @"iPad7,5"   :@264,
    @"iPad7,6"   :@264,
    // iPad Pro 3rd Gen 11-inch, 2018
    @"iPad8,1"   :@264,
    @"iPad8,3"   :@264,
    // iPad Pro 3rd Gen 11-inch 1TB, 2018
    @"iPad8,2"   :@264,
    @"iPad8,4"   :@264,
    // iPad Pro 3rd Gen 12.9-inch, 2018
    @"iPad8,5"   :@264,
    @"iPad8,7"   :@264,
    // iPad Pro 3rd Gen 12.9-inch 1TB, 2018
    @"iPad8,6"   :@264,
    @"iPad8,8"   :@264,
    // iPad Pro 4rd Gen 11-inch, 2020
    @"iPad8,9"   :@264,
    @"iPad8,10"  :@264,
    // iPad Pro 4rd Gen 12.9-inch, 2020
    @"iPad8,11"  :@264,
    @"iPad8,12"  :@264,
    // Mini 5
    @"iPad11,1"  :@326,
    @"iPad11,2"  :@326,
    // Air 3
    @"iPad11,3"  :@264,
    @"iPad11,4"  :@264,
    // 1st Gen
    @"iPod1,1"   :@163,
    // 2nd Gen
    @"iPod2,1"   :@163,
    // 3rd Gen
    @"iPod3,1"   :@163,
    // 4th Gen
    @"iPod4,1"   :@326,
    // 5th Gen
    @"iPod5,1"   :@326,
    // 6th Gen
    @"iPod7,1"   :@326,
    // 7th Gen
    @"iPod9,1"   :@326
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

- specify the curve quality: it forces the renderer to flatten the curves (i.e. generate polygon approximation) with more or less precision.
  `svgtConfigSet(SVGT_CONFIG_CURVES_QUALITY, <quality>)` where `quality` parameter ranges between 0 and 100: 0 represents the worst quality and 100 the best one.
- specify the system/user-agent language; this setting will affect the conditional rendering of `<switch>` elements and elements with `systemLanguage` attribute specified.
  `svgtLanguageSet(<language>, <script>, <region>, <variant>)` where `language`, `script`, `region`, `variant` strings follow the RFC5646 specification.
- specify font resources; this setting allows the library to acknowledge about an external font resource with its parameters (`font-family`, `font-weight`, `font-stretch`, `font-style`), that will be used to load font glyphs of a matching font during the rendering of SVG documents.
    `svgtFontResourceSet(<buffer>, <bufferSize>, <fontFamily>, <fontWeight>, <fontStretch>, <fontStyle>)`

```c
/*
    Configure parameters and thresholds for the AmanithSVG library.
    This function can be called at any time, but it will have effect only if:

    - the library has not been already initialized by a previous call to
      svgtInit, or
    - the library has been already initialized (i.e. svgtInit has been already
      called) but no other functions, except for svgtMaxCurrentThreads,
      svgtSurfaceMaxDimension, svgtFontResourceSet, have been called.

    When 'config' refers to an integer parameter, the given 'value' is converted
    to an integer using a mathematical floor operation.

    This function returns:

    - SVGT_ILLEGAL_ARGUMENT_ERROR if the library has been already initialized
      and one or more functions different than svgtMaxCurrentThreads,
      svgtSurfaceMaxDimension, svgtFontResourceSet, have been called

    - SVGT_ILLEGAL_ARGUMENT_ERROR if the specified value is not valid for the
      given configuration parameter

    - SVGT_NO_ERROR in all other cases (i.e. in case of success).
*/
SVGTErrorCode svgtConfigSet(SVGTConfig config,
                            SVGTfloat value);
```

```c
/*
    Get the current value relative to the specified configuration parameter.

    If the given parameter is invalid (i.e. it does not correspond to any
    value of the SVGTConfig enum type), a negative number is returned.
*/
SVGTfloat svgtConfigGet(SVGTConfig config);
```

```c
/*
    Set the system / user-agent language.
    Each part must follow the RFC5646 specification, in particular:

    - rfc5646Language must be a non-NULL 2 or 3 alpha characters (shortest
      ISO 639 code)
    - rfc5646Script could be NULL or a 4 alpha characters (ISO 15924 code)
    - rfc5646Region could be NULL or a 2 alpha or 3 digit characters
      (ISO 3166-1 code or UN M.49 code)
    - rfc5646Variant could be NULL or a registered variant

    This function can be called at any time, but it will have effect only if:

    - the library has not been already initialized by a previous call to
      svgtInit, or
    - the library has been already initialized (i.e. svgtInit has been already
      called) but no other functions, except for svgtMaxCurrentThreads,
      svgtSurfaceMaxDimension, svgtFontResourceSet, have been called.

    This function returns:

    - SVGT_ILLEGAL_ARGUMENT_ERROR if the library has been already initialized
      and one or more functions different than svgtMaxCurrentThreads,
      svgtSurfaceMaxDimension, svgtFontResourceSet, have been called

    - SVGT_ILLEGAL_ARGUMENT_ERROR if the given RFC5646 language part is NULL

    - SVGT_NO_ERROR in all other cases (i.e. in case of success).

    NB: the library starts with a default "EN" (English) language, no script,
    no region, no variant.
*/
SVGTErrorCode svgtLanguageSet(const char* rfc5646Language,
                              const char* rfc5646Script,
                              const char* rfc5646Region,
                              const char* rfc5646Variant);
```

```c
/*
    Get the maximum number of different threads that can "work" (e.g. create
    surfaces, create documents and draw them) concurrently.

    In multi-thread applications, each thread can only draw documents it has
    created, on surfaces it has created. In other words, a thread cannot
    draw a document created by another thread or on surfaces belonging to
    other threads.

    In order to get a valid value, the library must have been already
    initialized (see svgtInit).
*/
SVGTuint svgtMaxCurrentThreads(void);
```

```c
/*
    Instruct the library about an external font resource.
    All font resources must be specified in advance after the call to svgtInit
    and before to create/spawn threads.

    'buffer' must point to a read-only area containing an OpenType/TrueType
    font file. Such read-only buffer must be a valid and immutable memory area
    throughout the whole life of the application that uses the library.
    In multi-thread applications the 'buffer' memory must be accessible
    (readable) by all threads.
    
    This function returns:

    - SVGT_ILLEGAL_ARGUMENT_ERROR if 'buffer' is NULL or if 'bufferSize' is zero

    - SVGT_ILLEGAL_ARGUMENT_ERROR if 'fontWeight' is not a valid value from the
      SVGTFontWeight enumeration

    - SVGT_ILLEGAL_ARGUMENT_ERROR if 'fontStretch' is not a valid value from the
      SVGTFontStretch enumeration

    - SVGT_ILLEGAL_ARGUMENT_ERROR if 'fontStyle' is not a valid value from the
      SVGTFontStyle enumeration
    
    - SVGT_NO_ERROR if the operation was completed successfully
*/
SVGTErrorCode svgtFontResourceSet(const SVGTubyte* buffer,
                                  SVGTuint bufferSize,
                                  const char* fontFamily,
                                  SVGTFontWeight fontWeight,
                                  SVGTFontStretch fontStretch,
                                  SVGTFontStyle fontStyle);
```

## Finalization

Once finished to use AmanithSVG functionalities, it is advisable to call the `svgtDone` function, in order to be sure that all allocated SVG resources will be freed.

```c
/*
    Destroy the library, freeing all allocated resources.
    In multi-thread applications, this function must be called once, when all
    created/spawned threads have finished using the library functions.
*/
void svgtDone(void);
```
---
