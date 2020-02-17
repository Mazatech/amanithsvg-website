---
layout: default
title: "Load SVG files"
date: 2018-01-01 08:00:00 +0100
chapter: 5
categories: [api]
---

# Load SVG files

## Create an SVG document and load an SVG file

SVG defines vector-based graphics in [XML](https://www.w3.org/XML/) format (a markup language that defines a set of rules for encoding documents in a format which is both human-readable and machine-readable), so SVG files content can be represented as a C string.

Here is a standard C function able to load an SVG file and return a pointer to its content:

```c
char* loadXml(const char* fileName) {

    char* buffer;
    size_t size, read;
    FILE* fp = NULL;

#if defined(_MSC_VER) && (_MSC_VER >= 1400 )
    /* make Microsoft Visual Studio happy */
    errno_t err = fopen_s(&fp, fileName, "rb");
    if ((fp == NULL) || (err != 0)) {
#else
    fp = fopen(fileName, "rb");
    if (fp == NULL) {
#endif
        return NULL;
    }

    fseek(fp, 0, SEEK_SET);
    fgetc(fp);
    if (ferror(fp) != 0) {
        fclose(fp);
        return NULL;
    }

    /* get the file size */
    fseek(fp, 0, SEEK_END);
    size = ftell(fp);
    fseek(fp, 0, SEEK_SET);
    if (size == 0) {
        fclose(fp);
        return NULL;
    }

    /* allocate a memory buffer equal to the file size */
    buffer = (char *)malloc((size + 1) * sizeof(char));
    if (buffer == NULL) {
        fclose(fp);
        return NULL;
    }

    /* read the file content and store it within the memory buffer */
    read = fread(buffer, sizeof(char), size, fp);
    if (read != size) {
        free(buffer);
        fclose(fp);
        return NULL;
    }

    /* close the file and return the pointer to the memory buffer */
    buffer[size] = '\0';
    fclose(fp);
    return buffer;
}
```

```c
SVGTHandle loadSvg(const char* fileName) {

    SVGTHandle svgHandle = SVGT_INVALID_HANDLE;

    if (fileName != NULL) {
        /* allocate buffer and load file */
        char* buffer = (char *)loadXml(fileName);
        if (buffer != NULL) {
            /* create the SVG document */
            svgHandle = svgtDocCreate(buffer);
            /* free the allocated buffer */
            free(buffer);
        }
    }

    return svgHandle;
}
```

In order to render SVG contents, AmanithSVG must create an optimized in-memory representation of SVG files: a such representation is called an SVG document. It is possible to create an SVG document given its C string representation using the `svgtDocCreate` function:

```c
/*
    Create and load an SVG document, specifying the whole XML string.
    Return SVGT_INVALID_HANDLE in case of errors, else a valid document handle.
*/
SVGTHandle svgtDocCreate(const char* xmlText);
```

So, for example, in order to let AmanithSVG to render the userInterface.svg file, it is possible to create the relative SVG document as follow:

```c
SVGTHandle svgDoc = loadSvg("userInterface.svg");
```

When an SVG document is no longer necessary, its resources can be freed using the `svgtDocDestroyFunction`:

```c
/*
    Destroy a previously created SVG document.
    Return SVGT_NO_ERROR if the operation was completed successfully, else an
    error code.
*/
SVGTErrorCode svgtDocDestroy(SVGTHandle svgDoc);
```

SVG files themself optionally can provide information about the appropriate viewport region for the content via the `width` and `height` XML attributes on the outermost `<svg>` element. Once that an SVG document has been created, the following functions could be used to retrieve the suggested viewport width/height, in pixels:

```c
/*
    It returns -1 (i.e. an invalid width) in the following cases:
    - the library has not previously been initialized through the svgtInit
      function
    - outermost element is not an <svg> element
    - outermost <svg> element doesn't have a 'width' attribute specified
    - outermost <svg> element has a 'width' attribute specified
      in relative measure units (i.e. em, ex, % percentage)
*/
SVGTfloat svgtDocWidth(SVGTHandle svgDoc);
```

```c
/*
    It returns -1 (i.e. an invalid height) in the following cases:
    - the library has not previously been initialized through the svgtInit
      function
    - outermost element is not an <svg> element
    - outermost <svg> element doesn't have a 'height' attribute specified
    - outermost <svg> element has a 'height' attribute specified in
      relative measure units (i.e. em, ex, % percentage)
*/
SVGTfloat svgtDocHeight(SVGTHandle svgDoc);
```

---
