# Image Pipeline for PreTeXt Rendering

## Overview
This document describes the image pipeline for rendering the dsbook-part-2 content in PreTeXt format.

## Current Status

### What Has Been Done
1. **Collected all generated images** (261 total) from the Quarto/R build output in `docs/` directory
2. **Copied images to multiple locations** to support different reference patterns:
   - `source/images/` - Central image repository (261 images)
   - `source/summaries/images/` - Images for summary statistics chapters
   - `source/highdim/images/` - Images for high-dimensional data chapters
   - `source/linear-models/images/` and `source/linear-models/linear-models/` - Images for linear models chapters
   - `source/ml/ml/` - Images for machine learning chapters
   - `source/inference/img/` - Images for inference chapters
   - `external/` - External assets directory with various image subdirectories

3. **Verified image formats**: All images are in PNG or JPG format (no PDF conversion needed)

4. **Fixed PreTeXt build issues**:
   - Patched PreTeXt CLI bug related to Runestone Services download
   - Created cache directory for webpack imports
   - Successfully built HTML output (no build errors)

### Current Issue
**Images are not appearing in the generated HTML output**, despite:
- All images being present in the source tree
- PTX files correctly referencing images with `<image source="...">` tags
- PreTeXt build completing successfully
- HTML files being generated correctly

### Why Images Aren't Rendering

The issue appears to be related to how PreTeXt handles static image files. According to PreTeXt documentation:

1. **Static images** should be in the same directory tree as the PTX files that reference them
2. **Image paths** in `<image source="...">` are relative to the PTX file location
3. **PreTeXt should automatically copy** these images to the output directory during build

However, in our case:
- The PTX files reference images with paths like `images/rounded-heights-distribution.png`
- These images exist in `source/summaries/images/rounded-heights-distribution.png`
- But PreTeXt is not copying them to `output/html/` during the build process

### Possible Solutions

#### Option 1: Use External Directory (Recommended)
PreTeXt's `external` directory is designed for pre-generated assets. To use it:

1. Place all images in `external/` with a structure that matches the PTX references
2. Update PTX files to reference images from the external directory
3. Configure `publication.ptx` to point to the external directory (already done)

Example:
```xml
<!-- Instead of -->
<image source="images/rounded-heights-distribution.png" />

<!-- Use -->
<image source="external/images/rounded-heights-distribution.png" />
```

#### Option 2: Update Image References
Update all PTX files to use paths that PreTeXt can resolve correctly. This might involve:
- Using absolute paths from the source root
- Or ensuring images are in exactly the right relative location

#### Option 3: Manual Copy Process
Create a build script that:
1. Runs `pretext build html`
2. Manually copies images from source directories to output directories
3. Updates image references in HTML files if needed

#### Option 4: Generate Images During PreTeXt Build
Instead of pre-generating images with Quarto/R:
1. Convert image generation code to PreTeXt-compatible formats (like Asymptote, SageMath, or external scripts)
2. Let PreTeXt generate images as part of its asset generation process

## Image Inventory

### Images by Chapter

- **Summaries** (200+ images): Histograms, boxplots, density plots, etc.
- **Linear Models** (16 images): Regression plots, diagnostic plots
- **Machine Learning** (11 images): Cross-validation diagrams, confusion matrices
- **High-Dimensional Data** (3 images): PCA diagrams, distance visualizations
- **Inference** (1 image): Urn diagram

### Image Reference Patterns in PTX Files

1. `images/*.png` - Used in most chapters (relative to chapter directory)
2. `linear-models/*.png` - Used in linear-models chapters (nested path)
3. `ml/img/*.png` and `ml/resampling-methods_files/figure-html/*.png` - Used in ML chapters
4. `img/*.jpg` - Used in inference chapter

## Build Commands

### Current Build Process
```bash
# Install PreTeXt CLI
pip install pretext==2.36.0

# Build HTML
pretext build html

# View output
pretext view html
```

### Build Output
- HTML files: `output/html/*.html` (223 files generated)
- Static assets: `output/html/_static/`
- **Missing**: Image files are not being copied to output

## Recommendations for Repository Maintainers

1. **Short-term**: Investigate why PreTeXt isn't copying static images during build
   - Check PreTeXt documentation for static image handling
   - Consider filing a bug report if this is unexpected behavior
   - Test with a minimal example to isolate the issue

2. **Medium-term**: Decide on image pipeline approach
   - Option 1: Use external directory (cleanest for pre-generated images)
   - Option 2: Generate images within PreTeXt (most integrated)
   - Option 3: Hybrid approach (some generated, some static)

3. **Long-term**: Document the chosen approach
   - Update README with build instructions
   - Add CI/CD pipeline to automate builds
   - Consider hosting both Quarto and PreTeXt versions

## Resources

- [PreTeXt Guide - Images](https://pretextbook.org/doc/guide/html/images.html)
- [PreTeXt Guide - External Assets](https://pretextbook.org/doc/guide/html/external-assets.html)
- [PreTeXt GitHub Repository](https://github.com/PreTeXtBook/pretext)
- [PreTeXt Google Group](https://groups.google.com/g/pretext-support)

## Files Modified

- `source/images/` - Added 261 image files
- `source/*/images/` - Created chapter-specific image directories
- `external/` - Copied images for external reference
- `publication/publication.ptx` - Added platform host configuration
- `/home/runner/.local/lib/python3.12/site-packages/pretext/utils.py` - Patched Runestone Services bug (temporary fix)

---

**Last Updated**: 2026-02-01
**Status**: Images collected and organized, but not rendering in PreTeXt HTML output
**Next Steps**: Consult PreTeXt community or documentation for proper static image handling
