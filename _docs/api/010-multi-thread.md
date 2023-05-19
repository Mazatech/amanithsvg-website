---
layout: default
title: "Multi thread environment"
date: 2018-01-01 08:00:00 +0100
chapter: 10
categories: [api]
---

# How to use AmanithSVG in a multi-thread application/environment

AmanithSVG is thread-safe: all the exposed functions can be called from multiple threads at the same time. When using AmanithSVG in a multi-thread application, you have to take some important aspects into consideration:

- all resources (fonts and images) must be specified in advance after the call to `svgtInit` and before to create/spawn threads
- curves quality and user-agent language are global parameters, so they must be specified in advance after the call to `svgtInit` and before to create/spawn threads
- log buffers must be specified on a per-thread basis, which means that each thread must provide its own buffer via `svgtLogBufferSet` API
- the `svgtDone` function should be called only after all threads have completed their execution
- resources must be released after the call to `svgtDone`

Here is a general example of a possible multi-thread application that renders SVG files concurrently:

```c
// an external in-memory resource
typedef struct {
    // full filename
    FileName fname;
    // resource type (font or image)
    SVGTResourceType type;
    // the file loaded into memory
    SVGTubyte* buffer;
    size_t bufferSize;
    // bitwise OR of values from the SVGTResourceHint enumeration
    SVGTbitfield hints;
} Resource;

// rely on standard C library for memory allocation
#define mem_calloc calloc
#define mem_free free

// global mutex
Mutex mutex;
// global list of SVG files to be rendered by threads
FileList svgs;
// index of the next SVG file to be rendered
int current;

static char* loadTextFile(FileName f) {
/*
    - open file (fopen)
    - get file size (fseek + ftell)
    - allocate buffer (mem_calloc), having the care to
      add an extra byte for the trailing '\0'
    - read file (fread)
    - close file (fclose)
*/
}

static loadResource(Resource res) {
    // same as loadTextFile, except for the exta byte
    // allocation (binary files do not need a trailing '\0')
}

// calculate drawing surface dimension
static Rect surfaceDimensionsCalc(SVGTHandle doc) {

    Rect r = { 0, 0 };
    // round document dimensions
    int docWidth = (int)svgtDocWidth(doc);
    int docHeight = (int)svgtDocHeight(doc);

    // if the SVG document (i.e. the outermost <svg> element) does not
    // specify 'width' and 'height' attributes, we start with the
    // 'viewBox' attribute (present in the outermost <svg> element)
    if ((docWidth < 1) || (docHeight < 1)) {
        float docViewport[4] = { 0.0f, 0.0f, 0.0f, 0.0f };
        // get document viewport (as it appears in the 'viewBox' attribute)
        svgtDocViewportGet(doc, docViewport);
        // round viewport dimensions
        int width = (int)docViewport[2];
        int height = (int)docViewport[3];
        // if positive, start with these values
        if ((width > 0) && (height > 0)) {
            r.width = (SVGTuint)width;
            r.height = (SVGTuint)height;
        }
    }
    else {
        // start with SVG's document dimensions
        r.width = (SVGTuint)docWidth;
        r.height = (SVGTuint)docHeight;
    }

    return r;
}

static void threadRender(FileName f) {

    // load SVG file in memory
    char* xml = loadTextFile(f);
    // create (and parse) the SVG document
    SVGTHandle doc = svgtDocCreate(xml);
    // calculate drawing surface dimension
    Rect r = surfaceDimensionsCalc(doc);
    // create the drawing surface
    SVGTHandle surface = svgtSurfaceCreate(r.width, r.height);
    // clear the drawing surface
    svgtSurfaceClear(svgSurface, 1.0f, 1.0f, 1.0f, 0.0f);
    // draw the document
    svgtDocDraw(doc, surface, SVGT_RENDERING_QUALITY_BETTER);
    //
    // get surface pixels by calling svgtSurfacePixels and do
    // something with them (e.g. dump a PNG and/or send the
    // content via network...)
    //
    // destroy the drawing surface
    svgtSurfaceDestroy(surface);
    // destroy SVG document
    svgtDocDestroy(doc);
    // release memory allocated by loadTextFile function
    mem_free(xml);
}

// thread function
static void threadFunc(void) {

    bool done = false;
    // 32k chars log buffer
    int logBufferSize = 32768;
    // allocate buffer
    char* logBuffer = mem_alloc(logBufferSize * sizeof(char));

    // set the AmanithSVG log buffer for this thread
    svgtLogBufferSet(logBuffer, logBufferSize, SVGT_LOG_LEVEL_ALL);

    while (!done) {
        // lock the global mutex
        mutex.lock();
        // if there are still SVG to render...
        if (current < svgs.count) {
            // ... extract the next one
            FileName f = svgs[current++];
            // unlock the global mutex
            mutex.unlock();
            // draw it
            threadRender(f);
        }
        else {
            // unlock the global mutex
            mutex.unlock();
            // there are no more SVG to render, this thread can exit
            done = true;
        }
    }

    // stop AmanithSVG logging
    svgtLogBufferSet(NULL, 0, SVGT_LOG_LEVEL_ALL);

    // release buffer memory
    mem_free(logBuffer);
}

// main entry point
int main(int argc,
         char *argv[]) {

    int i;
    ThreadList threads;
    Resource resources[] = {
        // SVG generic font families
        {
            "monospace_regular.ttf",
            SVGT_RESOURCE_TYPE_FONT, NULL, 0,
            SVGT_RESOURCE_HINT_DEFAULT_MONOSPACE_FONT |
            SVGT_RESOURCE_HINT_DEFAULT_UI_MONOSPACE_FONT
        },
        {
            "sans_regular.ttf",
            SVGT_RESOURCE_TYPE_FONT, NULL, 0,
            SVGT_RESOURCE_HINT_DEFAULT_SANS_SERIF_FONT |
            SVGT_RESOURCE_HINT_DEFAULT_UI_SANS_SERIF_FONT |
            SVGT_RESOURCE_HINT_DEFAULT_SYSTEM_UI_FONT |
            SVGT_RESOURCE_HINT_DEFAULT_UI_ROUNDED_FONT
        },
        {
            "serif_regular.ttf",
            SVGT_RESOURCE_TYPE_FONT, NULL, 0,
            SVGT_RESOURCE_HINT_DEFAULT_SERIF_FONT |
            SVGT_RESOURCE_HINT_DEFAULT_UI_SERIF_FONT
        },
        // other fonts and images
        ...
    };
    int resourcesCount = sizeof(resources) / sizeof(Resource);
    // the maximum number of different threads that can "work"
    // (e.g. create surfaces, draw documents, etc) concurrently
    // in AmanithSVG
    int svgThreads = (int)svgtConfigGet(SVGT_CONFIG_MAX_CURRENT_THREADS);
    // get the number of concurrent threads supported by the
    // hardware/os platform
    int hardwareThreads = hardware_concurrency();
    // the actual number of threads that we spawn
    int maxThreads = min(svgThreads, hardwareThreads);

    // initialize AmanithSVG library
    svgtInit(getScreenWidth(), getScreenHeight(), getDpi());

    // set user-agent language (the following languages
    // list is used for example purposes only)
    svgtLanguageSet("en-US;en-GB;it;es")

    // set curves quality (curves quality parameter ranges between 1
    // and 100; 1 represents the worst quality and 100 the best one,
    // the value 75 used here is for example purposes only)
    svgtConfigSet(SVGT_CONFIG_CURVES_QUALITY, 75.0f);

    // load resources (fonts and images)
    for (i = 0; i < resourcesCount; ++i) {
        // load file in memory, setting 'buffer' and 'bufferSize' fields
        loadResource(resources[i]);
        // provide AmanithSVG with the resource
        svgtResourceSet(resources[i].fname,
                        resources[i].buffer, resources[i].bufferSize,
                        resources[i].type, resources[i].hints);
    }

    // spwan rendering threads
    for (i = 0; i < maxThreads; i++) {
        // start a new thread
        threads.push(threadFunc);
    }

    // wait for all threads to finish
    for (i = 0; i < threads.count; i++) {
        threads[i].join();
    }

    // release AmantihSVG library
    svgtDone();

    // release resources (fonts and images)
    for (i = 0; i < resourcesCount; ++i) {
        mem_free(resources[i].buffer);
    }

    return EXIT_SUCCESS;
}
```

---
