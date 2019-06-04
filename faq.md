# Frequently Asked Questions \(FAQ\)

## General questions

### Why?

The goal of `dcmqi` is to help you use DICOM for storing the results of quantitative image analysis.

Why would you want to use DICOM for your analysis results?

You should use DICOM if you want to improve interoperability of your data, to enhance the ability to automatically find and use the data by the computational tools, as well as support reuse of your data by individuals. These goals are widely recognized as important in the scientific community [\[1\]](http://dx.doi.org/10.1038/sdata.2016.18).

To highlight some of the specific advantages of using DICOM for storing analysis data, below we annotate the FAIR \(Findable, Accessible, Interoperable, Reusable\) Guiding Principles [\[1\]](http://dx.doi.org/10.1038/sdata.2016.18) formalized by [FORCE11](https://www.force11.org/group/fairgroup/fairprinciples), as applied to quantitative image analysis, describing how research formats help meet the FAIR requirements, and contrasting those with the functionality provided by DICOM.

While speaking of "research formats", we refer primarily to the formats commonly used by researchers developing quantitative image analysis tools. Examples include [NRRD](http://teem.sourceforge.net/nrrd/format.html), [MetaImage](http://www.itk.org/Wiki/MetaIO/Documentation), [NIfTI](http://nifti.nimh.nih.gov/nifti-1/), and [Analyze](http://www.grahamwideman.com/gw/brain/analyze/formatdoc.htm).

Notable example of a domain-specific solution proposed for data storage is [Brain Imaging Data Structure \(BIDS\)](https://www.nature.com/articles/sdata201644) being developed for neuroimaging applications. We are not aware of a domain-specific solution developed for cancer imaging.

| FAIR Guiding principle | Research formats | DICOM |
| :--- | :--- | :--- |
| **To be Findable:** |  |  |
| **F1.** \(meta\)data are assigned a globally unique and persistent identifier | usually not assigned | each object has a unique identifier |
| **F2.** data are described with rich metadata \(defined by R1 below\) | minimal metadata sufficient to solve specific task \(e.g., image resolution and orientation\) | metadata is stored in standardized attributes describing versatile aspects of the data \(the subject being imaged, processing details, references to related objects, etc.\) |
| **F3.** metadata clearly and explicitly include the identifier of the data it describes | metadata describing the subject may be stored separately from the analysis result, and cross-linked by means of file name or similar mechanism, creating opportunity for inconsistencies and errors | metadata is stored in the same object as the processing result |
| **F4.** \(meta\)data are registered or indexed in a searchable resource | problem-specific solutions | general-purpose search and indexing of DICOM data is supported by every Picture Archival and Communications System \(PACS\) using DICOM Query and Retrieve protocol, or using REST-based [DICOMWeb](https://dicomweb.hcintegrations.ca/) protocol |
| **To be Accessible:** |  |  |
| **A1.** \(meta\)data are retrievable, by their identifier using a standardized communication protocol | problem-specific solutions | general-purpose retrieval of DICOM data is supported by every Picture Archival and Communications System \(PACS\) using DICOM Query and Retrieve protocol, or using REST-based [DICOMWeb](https://dicomweb.hcintegrations.ca/) protocol |
| **A1.1.** the protocol is open, free, and universally implementable | no comparable protocols have been proposed and implemented in widely accessible solutions | yes |
| **A1.2.** the protocol allows for an authentication and authorization procedure, where necessary | no comparable protocols have been proposed and implemented in widely accessible solutions | DICOMWeb can be integrated with existing authentication protocols defined by other standards |
| **A2.** metadata is accessible, even when the data is no longer available | no | not applicable, since metadata is stored alongside the data in the same object |
| **To be Interoperable:** |  |  |
| **I1.** \(meta\)data use a formal, accessible, shared, and broadly applicable language for knowledge representation | domain-specific solutions | DICOM is a formal, accessible, shared and broadly used standard |
| **I2.** \(meta\)data use vocabularies that follow FAIR principles | domain-specific solutions | can reuse vocabularies defined elsewhere, relying often on the terminology defined by [SNOMED-CT](http://searchhealthit.techtarget.com/definition/SNOMED-CT), [UCUM](http://unitsofmeasure.org/trac), [NCIt](https://ncit.nci.nih.gov/ncitbrowser/), [FMA](https://bioportal.bioontology.org/ontologies/FMA), and allows for integration with other vocabularies, including those defined by the user |
| **I3.** \(meta\)data includes qualified references to other \(meta\)data | usually, no | derived objects can include pointers to the datasets used in the derivation, including the purpose of reference |
| **To be Reusable:** |  |  |
| **R1.** \(meta\)data are richly described with a plurality of accurate and relevant attributes | usually, no | data attributes that need to be included for a specific object are defined by the standard, as a result of the community discussion and consensus process; the process of amending the standard is formalized and open |
| **R2.** \(meta\)data are released with a clear and accessible data usage license | not applicable; data usage license is selected by the data provider | not applicable; data usage license is selected by the data provider; the DICOM standard itself is available free of charge, and its implementation is not restricted by any licenses |
| **R1.2.** \(meta\)data are associated with detailed provenance | usually, no | composite context is preserved across imaging and derived datasets describing patient and acquisition details; provenance-related attributes are included, depending on the specific object type |
| **R1.3.** \(meta\)data meet domain-relevant community standards | domain-specific solutions | DICOM is the main standard in the medical imaging domain |

\[1\] Wilkinson et al. 2016. The FAIR Guiding Principles for scientific data management and stewardship. Scientific data 3:160018. DOI: [10.1038/sdata.2016.18](http://dx.doi.org/10.1038/sdata.2016.18).

### Isn't DICOM used only for clinical images?

**No.**

**DICOM is** _**the**_ **international standard for biomedical images and image-related information.**

Widespread use of DICOM in clinic has led to a common mis-conception that DICOM is only suitable for storing clinical images. DICOM applications are **not** restricted to clinical use only. The standard defines objects for storing image and image-related data, which are perfectly suitable to support imaging _research_.

DICOM supports a wide range of biomedical applications, including preclinical, veterinary and small animal imaging. Preclinical small animal imaging was introduced to the standard in 2015, see [Supplement 187 Preclinical Small Animal Imaging Acquisition Context](ftp://medical.nema.org/medical/dicom/final/sup187_ft_preclinicalanimalacquisitioncontext.pdf).

Most common types of DICOM objects are those produced by the clinical imaging equipment: Computed Tomography \(CT\), Magnetic Resonance Imaging \(MRI\) or ultrasound \(US\) image objects.

Some of the examples of _image-related_ information that can be stored using DICOM include:

* radiation therapy dose, encoding dose distribution calculated by radiotherapy planning systems;
* radiation therapy structure set, containing contours of patient structures derived from images;
* segmentation image, which describes a classification of pixels in one or more referenced images;
* parametric map image, which contains pixels derived by the analysis of image data;
* measurements derived from an area in the image.

`dcmqi` provides tools to convert into standardized representation for the types of image-derived data produced in quantitative imaging research.

### Do you support DICOM Radiation Therapy Structure Sets \(RT-STRUCT\)?

`dcmqi` does not support conversion to/from DICOM RT-STRUCT objects.

DICOM RT-STRUCT is a type of DICOM object that found wide adoption in the radiation therapy community to store the planar contours of the anatomical structures prepared for the purposes of radiation dose planning. Due to the large installation base of radiation therapy planning tools and history of their usage, there are large amounts of image data annotated with DICOM RT-STRUCT contours.

Many of the segmentation algorithms produce classification of image voxels, and thus do not naturally suit the representation by means of planar contours, requiring extra conversion operation and a potential for information loss. DICOM Segmentation image encodes classification of individual image voxels directly, and is thus more natural fit for voxel-based segmentation results.

We do not support RT-STRUCT, since there are established dedicated tools and libraries for handling DICOM RT-STRUCT, with some of the most notable examples include [SlicerRT](http://slicerrt.github.io/) extension of [3D Slicer](http://slicer.org), [Plastimatch](http://plastimatch.org/), and [CERR](http://www.cerr.info/).

In our view, there is no need for yet another implementation of RT-STRUCT conversion to and from research formats.

We also note that although reporting of measurements for an image region defined by RT-STRUCT is supported by DICOM TID1500 Structured report, we currently do not implement direct support for this in `dcmqi`. We may consider adding this functionality in the future.

Users that have a need to report measurements over RT-STRUCT should first convert RT-STRUCT into a rasterized representation stored in an ITK-readable research format using any of the tools mentioned above, and then convert the result into DICOM Segmentation image using `dcmqi`. The measurements reported over the region defined by the Segmentation image can then be stored as a DICOM TID1500 Structured report.

## What are the formats that `dcmqi` supports?

The research formats we support are specific to the type of the object being converted, as summarized in the table below.

| Object type | Supported research formats | DICOM object |
| :--- | :--- | :--- |
| Segmentation image | All research volumetric image formats supported by the [Insight Toolkit \(ITK\)](https://itk.org/): [NRRD](http://teem.sourceforge.net/nrrd/format.html), [MetaImage](http://www.itk.org/Wiki/MetaIO/Documentation), [NIfTI](http://nifti.nimh.nih.gov/nifti-1/), [Analyze](http://www.grahamwideman.com/gw/brain/analyze/formatdoc.htm); extra metadata is communicated using [JSON](http://www.json.org/) and constrained by [JSON-Schema](http://json-schema.org/). | [DICOM Segmentation Image](http://dicom.nema.org/medical/dicom/current/output/chtml/part03/sect_A.51.html) |
| Parametric map image | All research volumetric image formats supported by the [Insight Toolkit \(ITK\)](https://itk.org/): [NRRD](http://teem.sourceforge.net/nrrd/format.html), [MetaImage](http://www.itk.org/Wiki/MetaIO/Documentation), [NIfTI](http://nifti.nimh.nih.gov/nifti-1/), [Analyze](http://www.grahamwideman.com/gw/brain/analyze/formatdoc.htm); extra metadata is communicated using [JSON](http://www.json.org/) and constrained by [JSON-Schema](http://json-schema.org/). | [DICOM Parametric map ](http://dicom.nema.org/medical/dicom/current/output/chtml/part03/sect_A.75.html) |
| Volumetric measurements | Both measurements and associated metadata should be described using [JSON](http://www.json.org/) and constrained by [JSON-Schema](http://json-schema.org/). We are are planning to add support for [CSV](https://en.wikipedia.org/wiki/Comma-separated_values) as input format in the future. | [DICOM TID1500 Structured Report](http://dicom.nema.org/medical/dicom/current/output/chtml/part16/chapter_A.html#sect_TID_1500) |

### How is `dcmqi` related to ...?

Below is the list of various tools that are used by medical imaging researchers, and description of how they relate to `dcmqi`:

* [Insight Toolkit](https://itk.org) - does not provide tools for conversion of DICOM objects supported by `dcmqi`. `dcmqi` uses ITK as a lower-level component for reading and writing research formats, and for image data operations.
* [DCMTK](http://dcmtk.org), [GDCM](https://sourceforge.net/projects/gdcm/) - provide attribute- and SR tree-level C++ API for interacting with DICOM data; provide general-purpose command line tools for converting DICOM objects into human-readable list of attribute, and editing individual attributes of a DICOM dataset; do not provide tools for generating DICOM objects from research formats. `dcmqi` is using DCMTK as the lower-level component to operate on DICOM data.
* [PixelMed](http://www.pixelmed.com/dicomtoolkit.html) Toolkit - provides attribute- and module-level Java API for interacting with DICOM objects; does not provide conversion tools for generating DICOM Segmentation image objects, Parametric maps, or volumetric measurements reports from research formats
* [dicom3tools](http://www.dclunie.com/dicom3tools.html) - provides attribute-level C API for interacting with DICOM objects, and general-purpose DICOM editing tools that can be used to change individual attributes of the dataset. Does not provide conversion tools for generating DICOM Segmentation image objects, Parametric maps, or volumetric measurements reports from research formats
* [pydicom](http://www.pydicom.org/) - provides attribute-level Python API for interacting with DICOM objects; does not provide conversion tools for generating DICOM Segmentation image objects, Parametric maps, or volumetric measurements reports from research formats
* [3D Slicer](http://slicer.org) - provides interactive application to load and process DICOM data; includes `dcmqi` as an extension; uses `dcmqi` to perform conversion of the objects `dcmqi` supports.
* [ePAD](https://epad.stanford.edu/) - provides interactive application to visualize and annotate DICOM data; by design, does not provide conversion tools; uses `dcmqi` to perform conversion of DICOM TID1500 SR objects supports; uses attribute-level API of [PixelMed](http://www.pixelmed.com/dicomtoolkit.html) to implement support of DICOM Segmentation objects and DICOM Parametric maps.
* [CERR](http://www.cerr.info/) - a software platform for developing and sharing research results in radiation therapy treatment planning. Supports DICOM RT-STRUCT annotation format. Does not support DICOM Segmentation image, Parametric map, or volumetric measurements.

### Can I use `dcmqi` for patient care?

`dcmqi` is intended for research work and _has no FDA clearances or approvals of any kind_. It is the responsibility of the user to comply with all laws and regulations \(and moral/ethical guidelines\) when using `dcmqi`.

## Usage related questions

### Conversion fails with the "Missing Attribute(s)" error - what should I do?

Image-derived objects, such as segmentations or parametric map, inherit some of the attributes from the image
they were derived from. Such attributes include, for example, those from the "General Study" and "Patient" modules.
In the situation where some of those attributes that are mandatory, but are not present in the source images, converter
will fail. The reason for this is that the converter will not write out an invalid object, and a valid object cannot
be constructed when mandatory attributes are missing.

To resolve this situation, you can patch the source image to add the missing attributes. To do such patching we recommend the DCMTK [`dcmodify`](https://support.dcmtk.org/docs/dcmodify.html) tool.

As an example, here is an error that would be generated by the `itkimage2segimage` converter when a mandatory "CodingSchemeDesignator" attribute is missing in the source DICOM image:

```
W: CodingSchemeDesignator (0008,0102) absent in CodeSequenceMacro (type 1)
1 of 512 slices mapped to source DICOM images
Found 3 label(s)
Skipping label 0
Processing label 1
Total non-empty slices that will be encoded in SEG for label 1 is 512
 (inclusive from 0 to 512)
Processing label 2
Total non-empty slices that will be encoded in SEG for label 2 is 512
 (inclusive from 0 to 512)
E: CodingSchemeDesignator (0008,0102) absent in CodeSequenceMacro (type 1)
E: Could not write item #0 in ProcedureCodeSequence: Missing Attribute(s)
FATAL ERROR: Writing of the SEG dataset failed! Please report the problem to the developers, ideally accompanied by a de-identified dataset allowing to reproduce the problem!
ERROR: Conversion failed.
```

Examining the content of the source image, indeed this attribute is missing, but [is mandatory](http://dicom.nema.org/medical/dicom/current/output/chtml/part03/sect_8.8.html#table_8.8-1a):

```
(0008,1032) SQ (Sequence with explicit length #=1)      #  72, 1 ProcedureCodeSequence
  (fffe,e000) na (Item with explicit length #=3)          #  64, 1 Item
    (0008,0100) SH [M2197]                                  #   6, 1 CodeValue
    (0008,0103) SH [0]                                      #   2, 1 CodingSchemeVersion
    (0008,0104) LO [BWH MR PELVIS WWO CONTRAST M2197]       #  32, 1 CodeMeaning
```

To fix this issue, the following command can be used:

```
dcmodify -i "ProcedureCodeSequence[0].CodingSchemeDesignator=99UNKNOWN" 000000.dcm
```

Note the "99" prefix used for the coding scheme designator. Unless you know which coding scheme the specific code
belongs to, you should always use this prefix to indicate that the coding scheme is "private" (not one of [the coding
schemes defined for use in the DICOM standard](http://dicom.nema.org/medical/dicom/current/output/chtml/part16/chapter_8.html)).
