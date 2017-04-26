# Multi-structure segmentation of the brain

## Summary of the use case

Segmentations of
* grey and white matter (labels 4 and 5 in [this NIfTI file](http://slicer.kitware.com/midas3/download/item/285812/gmwm-dcmspace.nii.gz))
* left and right cerebral hemispheres (labels 4 and 5 in [this NIfTI file](http://slicer.kitware.com/midas3/download/item/285813/lhrh-dcmspace.nii.gz))
* overall brain mask (label 1 in [this NIfTI file](http://slicer.kitware.com/midas3/download/item/285811/brainmask-dcmspace.nii.gz))

were produced by FreeSurfer by segmenting [this DICOM MR series](http://slicer.kitware.com/midas3/download/item/285806/CMET-MRhead.zip).

## Conversion to DICOM SEG

### Visualization of the input data

As a first step, it is always a good idea to visualize the input data. We recommend (3D Slicer)[http://slicer.org] for visualizing both DICOM and research format data.

Import DICOM dataset into Slicer DICOM database by unzipping the downloaded file with the MR series, opening DICOM Browser, and importing the directory with the DICOM MR series files (choose "Add link" when prompted).

![](/use_cases/dicom_import.jpg)

Next, load segmentations stored in NIfTI. To do that, open "Add data" 

## Advantages offered by using DICOM SEG

## Potential issues

