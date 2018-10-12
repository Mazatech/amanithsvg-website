---
layout: default
title: "Initialization and finalization"
date: 2018-01-01 08:00:00 +0100
chapter: 4
categories: [api]
---

# Initialization and finalization

Before to use any functionality, AmanithSVG must be initialized calling `svgtInit` function:

```c
// Initialize the library.
// Return SVGT_NO_ERROR if the operation was completed successfully, else an error code.
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
NSScreen *screen = [NSScreen mainScreen];
NSDictionary *description = [screen deviceDescription];
NSSize displayPixelSize = [[description objectForKey:NSDeviceSize] sizeValue];
CGSize displayPhysicalSize = CGDisplayScreenSize([[description objectForKey:@"NSScreenNumber"] unsignedIntValue]);
screenWidth = displayPixelSize.width;
screenHeight = displayPixelSize.height;
dpi = ((displayPixelSize.width / displayPhysicalSize.width) * 25.4f);
```

### Linux X11

```c
Display* display = XOpenDisplay(NULL);
int screen = DefaultScreen(display);
screenWidth = DisplayWidth(display, screen);
screenHeight = DisplayHeight(display, screen);
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
CGRect screenRect = [[UIScreen mainScreen] bounds];
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
    @"iPod7,1"   :@326
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

Once finished to use AmanithSVG functionalities, it is advisable to call the `svgtDone` function, in order to be sure that all allocated SVG resources will be freed.

```c
// Destroy the library, freeing all allocated resources.
void svgtDone(void);
```
---
