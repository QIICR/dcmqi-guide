# Segmentations and measurements from prostate MRI

## Summary of the use case

In this use case we will summarize the approach to encode segmentations of various structures and measurements derived using those segmentations from multi-parametric Magnetic Resonance Imaging (MRI) of the prostate.

More specifically, we will discuss encoding of the imaging-derived data discussed in the following paper:

> Fedorov A, Vangel MG, Tempany CM, Fennessy FM. Multiparametric Magnetic Resonance Imaging of the Prostate: Repeatability of Volume and Apparent Diffusion Coefficient Quantification. _Investigative Radiology_; 2017;52(9):538–546 http://dx.doi.org/10.1097/RLI.0000000000000382.

The imaging-derived data discussed in that paper consists of the two components:

1. Segmentations of the following structures:
 * Whole gland of the prostate
 * Peripheral zone of the prostate
 * Suspected tumor area of the prostate within the peripheral zone of the prostate
 * Normal-appearing area of the peripheral zone

2. Segmentation-based measurements: the manuscript is concerned with evaluating the repeatability of the
 * mean volume of the regions listed above, and
 * mean Apparent Diffusion Coefficient (ADC) values calculated over the segmentation-defined regions.

## Conversion to DICOM SEG





