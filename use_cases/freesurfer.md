# Multi-structure segmentation of the brain

## Summary of the use case

Segmentations of
* grey and white matter (labels 4 and 5 in [this NIfTI file](http://slicer.kitware.com/midas3/download/item/285812/gmwm-dcmspace.nii.gz))
* left and right cerebral hemispheres (labels 2 and 3 in [this NIfTI file](http://slicer.kitware.com/midas3/download/item/285813/lhrh-dcmspace.nii.gz))
* overall brain mask (label 1 in [this NIfTI file](http://slicer.kitware.com/midas3/download/item/285811/brainmask-dcmspace.nii.gz))

were produced by FreeSurfer by segmenting [this DICOM MR series](http://slicer.kitware.com/midas3/download/item/285806/CMET-MRhead.zip).

## Conversion to DICOM SEG

### Visualization of the input data

As a first step, it is always a good idea to visualize the input data. We recommend [3D Slicer](http://slicer.org) for visualizing both DICOM and research format data.

Import DICOM dataset into Slicer DICOM database by unzipping the downloaded file with the MR series, opening DICOM Browser, and importing the directory with the DICOM MR series files (choose "Add link" when prompted).

![](/use_cases/dicom_import.jpg)

Next, load segmentations stored in NIfTI. To do that, open "Add data", select the NIfTI segmentation files, check "Options" flag, and check "LabelMap" for each of the files in the list. This will tell Slicer that the files you are loading contain segmentations.

![](/use_cases/load_labels.jpg)

You should be able to see image with the segmentation for one of the labels shown in the overlay. If you mouse over the little pin icon in the corner of the slice viewer (orange arrow in the screenshot below), then expand the popup panel (red arrow), you can select each of the label files in the label selector (green arrow).

![](/use_cases/select_overlay.jpg)

To figure out the value for the individual labels in the overlay, you can mouse over the label, and then look in the Data Probe panel in the bottom left corner of the Slicer window (red arrow in the screenshot below).

![](/use_cases/data_probe.jpg)

Using the hints discussed above, we can figure out that:
* 1 = overall brain mask
* 2 = left hemisphere
* 3 = right hemisphere
* 4 = gray matter
* 5 = white matter

![](/use_cases/freesurfer-usecase-all_labels.jpg)

We will need the information above in the process of data conversion!

## Advantages offered by using DICOM SEG

## Potential issues

