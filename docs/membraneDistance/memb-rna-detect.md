# RNA detection in smFISH images

Two options exist, you can either directly analyze the smFISH channels in FQ to
detect individual RNAs, ~~or you can use a Matlab script that detect individual RNA
molecule but also decomposes RNA aggregates into individual RNAs with a Gaussian
Mixture Model (GMM)~~.

## Detection of individual RNAs

Below only a quick summary of the workflow is presented. For more details consult
the [FQ manual](<>).

1.  Open FQ
2.  Set analysis folder to folder containing the image that should be analyzed:
    `Menu Folder` > `Set root folder`)
3.  Open image that should be analyzed.
4.  Draw outline of embryo / cells:
    1.  Open dedicated interface: 	Button	`Define outlines`
    2.  Draw a new cell: Button	`New cell`
    3.  Save outline: Button	`Quick-save`
    4.  Return to main FQ: Button 	`Finished`
5.  Filter image (default filter with `LoG` is usually good).
6.  Inspect image. On the right part of the interface select `Filtered image`,
    disable `outline`, and double click on the image. This will show a maximum
    intensity projection of the filtered image in a separate window. Here, you can
    change the contrast and zoom. In order to determine an appropriate threshold
    in the next step, determine a generous range of intensities range corresponding
    to the individual RNA molecules.
7.  Set **pre-detection settings**. FQ will test how many RNAs are detected with a
    range of user-defined intensity thresholds (based on the manual inspection of
    the image from above).
        1. Open dedicated interface:	Button `Detect`
        0. In the user-dialog change the first two parameter: minimum and maximum
           threshold to test. We usually use a minimum value that’s somewhat lower
           than the lowest RNAs to also consider intensity value corresponding to background.
        0. FQ will calculate the number of detected RNAs for a range of values in
           this interval. Depending on the size of the image, this can take a little while. Once done, a plot with the number of detected RNAs as a function of the different thresholds is shown. If the specified range is not appropriate, it can be changed in this interface the computation be repeated.
         0. Ideally, this curve shows a plateau for a value range corresponding to a
           good detection threshold. If that’s not the case, a reasonable value
           corresponding to a visual assessment of what individual RNAs are should be chosen. A pre-detection with a given detection threshold can be perform and the results be inspected in a separate window.

        One other parameter that could be important to adjust is the **cropping region** around each RNA that is considered for analysis and fitting. Reducing this size allows sometimes to better detect closely spaced RNAs but at the cost of an  imprecision in the fit. For most applications a value of +/- 2 voxels in XY and Z are a good compromise.

8.  Once you are satisfied with the settings press on `Perform detection for all cells` to return the main window.
9.  Press button `Fit` to fit each detected spot with a 3D Gaussian function.
10. [Optional]. You can set threshold values for the different fitting parameters,
    e.g. remove spots with very small or very large standard deviations.
11. You can now save the detection settings: `[FQ] Main`>`Save`>`Save detection settings`.
    As a file-name specify `FQ__settings_MATURE.txt`
12. Depending on which analysis you want to perform, you either save the detection
    results or only the specified cell outline
13. If you are satisfied with these detection results, you can save them directly:  `[FQ] Main`>`Save`>`Save ???`
14. If you would like to also analyze RNA aggregates, you can save only the outline: `[FQ] Main`>`Save`>`Save ???`

## ~~Detection of clustered RNAs~~ **TODO**

~~After performing the steps above you will have two text files (1) the cell outline,
(2) FQ detection settings. We provide a Matlab script `WRAPPER_analyze_smFISH__EXP_v1.m`
which detects individual RNAs and then decomposes aggregates into individual RNAs
(for more information see our [Nat Communications paper](ToBeAdded)). This
script allows batch-processing and multiple images can be analyzed together, where image is processed with its dedicated outline and settings. This can be done with the Matlab script. This script recursively searches a user-defined parental folder for folders containing FQ outline and settings files. These folders will then be processed and RNA detection performed, sub-folders will containing different analysis results will be created.~~

-   `results_GMM` contains the RNA detection with the GMM (to decompose foci into
    individual RNAs). The most important files are the text files ending
    with `_res_GMM.txt` containing the RNA detection results, which will be used
    to perform the co-localization. The folder further contains a subfolder `#plots`
    with a PDF showing the results of the GMM detection. Individual RNAs are
     shown in green, decomposed foci with a red number indicating how many RNAs
    were estimated to be in each foci.
-   `results_noGMM` contains the RNA detection results without the GMM.

**Note**. The script also computes localization features that are not necessary
for this analysis. To avoid this computation you can specify the line where
the parameters for this analysis are set toS
