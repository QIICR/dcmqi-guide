# 
 







# Frequently Asked Questions (FAQ)

## Why? {#why}

The goal of `dcmqi` is to help you use DICOM for storing the results of quantitative image analysis.

Why would you want to use DICOM for your analysis results?

You should use DICOM if you want to improve interoperability of your data, to enhance the ability to automatically find and use the data by the computational tools, as well as support reuse of your data by individuals [1].


[1] 


## Isn't DICOM used only for clinical images? {#clinicalonly}

**No.**

**DICOM is _the_ international standard for biomedical images and image-related information.** It is a common mis-conception to think that DICOM is only suitable for storing clinical images, since these types of DICOM objects are most common and are widespread in the clinic.

DICOM is applicable not only in clinical care, but also in clinical and pre-clinical research.

DICOM supports a wide range of biomedical applications, including preclinical, veterinary and small animal imaging. Preclinical small animal imaging was introduced to the standard in 2015, see [Supplement 187 Preclinical Small Animal Imaging Acquisition Context](ftp://medical.nema.org/medical/dicom/final/sup187_ft_preclinicalanimalacquisitioncontext.pdf).

Most common types of DICOM objects are those produced by the clinical imaging equipment: Computed Tomography (CT), Magnetic Resonance Imaging (MR) or ultrasound (US) image objects. 

Some of the examples of _image-related_ information that can be stored using DICOM include:
* radiation therapy dose, encoding dose distribution calculated by radiotherapy planning systems;
* radiation therapy structure set, containing contours of patient structures derived from images;
* segmentation image, which describes a classification of pixels in one or more referenced images;
* parametric map image, which contains pixels derived by the analysis of image data;
* measurements derived from an area in the image.

`dcmqi` provides tools to convert into standardized representation for the types of image-derived data produced in quantitative imaging research.

## How about DICOM Radiation Therapy Structure Sets (RT-STRUCT)? {#rtstruct}

## What are the research formats that `dcmqi` can convert into DICOM? {#formats}