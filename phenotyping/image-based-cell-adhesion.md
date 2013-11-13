% An Image Based Cell Adhesion Assay
% Paul Magwene
% Last updated: 13 November 2013


### Goal

Measure ability of yeast strains to adhere to plastic (polystrene). Plastic adhesion is a commonly used assay in studies of microbial biofilms.  The measurement goal of the image analys described below is to accruately estimate the areal fraction of the bottom of a polystrene well that are covered by adherent cells.  

**Source of images:** Yeast cultures are grown overnight in liquid culture in round bottom, polystrene 96-well plates.  Growth media and non-adherent cells are aspirated from the wells, followed by several gentle rinses.  The center of each well is imaged using brightfield microscopy on an inverted or upright scope with a 10x objective.  Images are saved in a lossy image format such as TIFF.


## Assay Steps

1. Open image in Fiji 
    * `File > Open`

2. If image is not already grayscale, convert to 8-bit grayscale
    * `Image > Type > 8-bit`

3. Use rectangular selection tool to select the approximate region of the image that is in focus -- choose rectangle slightly larger (say 10% on each side) than region in focus

4. Use copy command (command-c (OSX) or ctrl-c (Windows)) to copy selected region to internal clipboard

5. Create two new images from the selected region
    * `File > New > Internal Clipboard`  (repeat this twice)

6. Choose one of the two images to become your binary image. We'll refer to this as the "derived image", the other image we'll refer to as the "base image".

7.  With window of the derived image selected:
    * `Process > Subtract Background` -- try rolling ball radius of 10 or 15 pixels as a starting point
        - Goal: eliminate variation in background lighting

    * `Process > Enhance Contrast` -- Set saturated pixels ~ 0.5%, select normalize and equalize options.
        - Goal: make edges of cells more distinct
    
    * `Process > Filters > Gaussian Blur` -- Starting with Sigma (Radius) of ~5 pixels (use preview to help choose)
        - alternately start with a lower radius (e.g. 2 or 3) and apply filter multiple times
        - Goal: "fill in" holes in outlines of cells, but NOT fill in spaces between cells
    
    * `Process > Binary > Make Binary`
        - Goal: turn grayscale image to black and white image

8. Compare your B&W derived image to the base image
    * Select the grayscale "base image"

    * `Image > Overlay > Add Image...`

        - In "Image to add" chose the window that has your B&W derived image
        - Set the Opacity to 50%

    * Visually evaluate the dark areas of the derived image to the base image.  Do the dark areas of the derived image provide a reasonable mask to the cells in the base image?  If so, proceed to the next step.

9. Select the B&W derived image window
    * Pick the elliptical selection tool from the Fiji toolbar
    
    * In the B&W derived image pick the circular subregion of the image that corresponded to the region of the well that was in focus.

    * `Analyze > Set Measurements` 
        - Choose "Area" and "Area Fraction" boxes

    * `Analyze > Analyze Particles...`
        - Set Size:  25 - Inifinity (choice of minimum will depend on actual resolution image was captured at)
        - Select "Clear Results" and "Summarize"
        - Hit `OK` and this function will return a summary window giving areal percentage of selected region that is filled with black.  This percentage is your estimate of the fraction of surface covered by cells.

10. Repeat the above steps for each of your images of interest.



## Notes on statistical analysis 

Percentage (proportion) data is usually not normally distributed.  To make such data better conform to assumptions of normality one can use the "arscine transform":

$$
y = \sin^{-1}(\sqrt{p})
$$

where $p$ is the percentage expressed as a fraction (i.e. 57% = 0.57).

See the following web page for a discussion of data transformation:

- http://rfd.uoregon.edu/files/rfd/StatisticalResources/arcsin.txt
- http://udel.edu/~mcdonald/stattransform.html