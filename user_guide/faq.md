# 
 







# Frequently Asked Questions (FAQ)

## Why?

The goal of `dcmqi` is to help you use DICOM for storing the results of quantitative image analysis.

Why would you want to use DICOM for your analysis results?

You should use DICOM if you want to improve interoperability of your data, to enhance the ability to automatically find and use the data by the computational tools, as well as support reuse of your data by individuals. These goals are widely recognized as important in the scientific community [[1]](http://dx.doi.org/10.1038/sdata.2016.18).

To highlight some of the specific advantages of using DICOM for storing analysis data, below we annotate the FAIR (Findable, Accessible, Interoperable, Reusable) Guiding Principles [[1]](http://dx.doi.org/10.1038/sdata.2016.18) formalized by [FORCE11](https://www.force11.org/group/fairgroup/fairprinciples), as applied to quantitative image analysis and functionality provided by DICOM.

FAIR Guiding principle | Research formats | DICOM 
-|-|-
**To be Findable:**|||
**F1.** (meta)data are assigned a globally unique and persistent identifier|usually not assigned|each object has a unique identifier|
**F2.** data are described with rich metadata (defined by R1 below)|minimal metadata sufficient to solve specific task (e.g., image resolution and orientation)|metadata is stored in standardized attributes describing versatile aspects of the data (the subject being imaged, processing details, references to related objects, etc.)
**F3.** metadata clearly and explicitly include the identifier of the data it describes | metadata describing the subject may be stored separately from the analysis result, and cross-linked by means of file name or similar mechanism, creating opportunity for inconsistencies and errors| metadata is stored in the same object as the processing result
**F4.** (meta)data are registered or indexed in a searchable resource|problem-specific solutions|general-purpose search and indexing of DICOM data is supported by every Picture Archival and Communications System (PACS) using DICOM Query and Retrieve protocol, or using REST-based [DICOMWeb](https://dicomweb.hcintegrations.ca/) protocol
**To be Accessible**|||
**A1.** (meta)data are retrievable, by their identifier using a standardized communication protocol|problem-specific solutions|general-purpose retrieval of DICOM data is supported by every Picture Archival and Communications System (PACS) using DICOM Query and Retrieve protocol, or using REST-based [DICOMWeb](https://dicomweb.hcintegrations.ca/) protocol
**A1.1.** the protocol is open, free, and universally implementable|no comparable protocols have been proposed and implemented in widely accessible solutions|yes
**A1.2.** the protocol allows for an authentication and authorization procedure, where necessary|no comparable protocols have been proposed and implemented in widely accessible solutions|DICOMWeb can be integrated with existing authentication protocols defined by other standards
**A2.** metadata is accessible, even when the data is no longer available|



[1] Wilkinson et al. 2016. The FAIR Guiding Principles for scientific data management and stewardship. Scientific data 3:160018. DOI: [10.1038/sdata.2016.18](http://dx.doi.org/10.1038/sdata.2016.18).


## Isn't DICOM used only for clinical images?

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

## How about DICOM Radiation Therapy Structure Sets (RT-STRUCT)? 

## What are the research formats that `dcmqi` can convert into DICOM? 