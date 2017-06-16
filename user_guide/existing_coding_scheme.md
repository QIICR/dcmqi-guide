# Using DICOM defined coding schemes

In DICOM, the process of choosing a code, and a coding scheme, depends on the context. In the following, we will discuss some of the guidelines that can be used to choose suitable codes for the tasks of segmentation, measurements and parametric map conversion supported by `dcmqi`.

* [Segmentation](#segmentation)
* [Measurements](#measurements)
* [Parametric map](#parametric-map)

## Segmentation

While converting segmentations, you will need to define the following coded entities in the JSON file:

* SegmentedPropertyCategoryCodeSequence
* SegmentedPropertyTypeCodeSequence
* SegmentedPropertyTypeModifierCodeSequence (when applicable)
* AnatomicRegionSequence (when applicable)
* AnatomicRegionModifierSequence (when applicable)

This looks overwhelming indeed! That's why we developed [a web application](http://qiicr.org/dcmqi/#/seg) to help you interactively choose the codes for each of those items. If you want to know the details, read on!

For each of these attributes, DICOM provides guidance on the selection of the suitable codes.

**SegmentedPropertyCategoryCodeSequence** codes are listed in context group  ID (CID) [CID 7150](http://dicom.nema.org/medical/dicom/current/output/chtml/part16/sect_CID_7150.html) (extensible, i.e., you are not forced to use only the codes from this selection)

**SegmentedPropertyTypeCodeSequence** codes are defined in [CID 7151](http://dicom.nema.org/medical/dicom/current/output/chtml/part16/sect_CID_7151.html), which points to the application-specific CIDs that you can follow for the lists of codes

**SegmentedPropertyTypeModifierCodeSequence** is an optional attribute that augments segmented property type code. As an example, if SegmentedPropertyTypeModifier is "Kidney", SegmentedPropertyTypeModifier can be "Left" to specify laterality. More modifier codes are available in [part 16 of the standard](http://dicom.nema.org/medical/dicom/current/output/chtml/part16/PS3.16.html), as an example see [CID 2 Anatomic Modifier](http://dicom.nema.org/medical/dicom/current/output/chtml/part16/chapter_B.html#table_CID_2).

**AnatomicRegionSequence** and its modifier do not always have to be specified. In some situations, information contained in SegmentedPropertyType is sufficient. E.g., if one is creating an atlas, where the properties are purely anatomical, and there is no more to say about them than the anatomy, then the anatomy goes in the Segmented Property Type Code Sequence. If one has different types of properties (e.g., necrosis, enhancing rim, gross tumor volume), but one wants to say something about the anatomy (e.g., where the tumor is at), then the property goes in Segmented Property Type Code Sequence and the anatomy goes in Anatomic Region Sequence. When you do want to specify AnatomicRegion, you can consult [CID 4 Anatomic Region](http://dicom.nema.org/medical/dicom/current/output/chtml/part16/sect_CID_4.html) for the list of codes. Modifier for this code is specified as needed, following the same approach as discussed for SegmentedPropertyTypeModifierCodeSequence. 

[The web application](http://qiicr.org/dcmqi/#/seg) we mentioned earlier provides an interactive interface to somewhat simplify the task of populating the codes discussed above.

### SNOMED CT

Most, if not all codes listed in the contexts referenced earlier are from SNOMED CT, and have DICOM CodingSchemeDesignator `SRT`. The reason for this is that [Systematized Nomenclature of Medicine Clinical Terms (SNOMED® CT)](https://en.wikipedia.org/wiki/SNOMED_CT) coding scheme is the preferred controlled terminology used by DICOM . Most, if not all, of the codes used to define the entities above are from SNOMED CT. SNOMED CT maintains a systematically organized computer processable collection of medical terms providing codes, terms, synonyms and definitions used in clinical documentation and reporting. SNOMED CT maintains the hierarchy of relationships among the codes, which can be used for semantic reasoning on the data. As an example, see [hierarchy of the entities related to the Liver code in the SNOMED CT Browser](http://browser.ihtsdotools.org/?perspective=full&conceptId1=181268008&edition=en-edition&release=v20160731&server=http://browser.ihtsdotools.org/api/snomed&langRefset=900000000000509007).

### SNOMED CT License exemption

Note that SNOMED CT codes included in the DICOM standard are exempt from [SNOMED CT license](http://www.snomed.org/snomed-ct/get-snomed-ct). The details are discussed in [this blog post](http://dclunie.blogspot.com/2016/03/dicom-and-snomed-back-in-bed-together.html). In short:

> Users and commercial and open source DICOM developers can be reassured that they may continue to use the subset of SNOMED concepts in the DICOM standard in their products and software, globally and without a fee or individual license.

## Measurements

**TBD - work in progress**

## Parametric map

The following codes can be passed to describe the parametric map you are converting using [itkimage2paramap converter](https://qiicr.gitbooks.io/dcmqi-guide/content/user_guide/itkimage2paramap.html):

* QuantityValueCode: Quantity being measured at each pixel - select code from [TID 7180](http://dicom.nema.org/medical/dicom/current/output/chtml/part16/sect_CID_7180.html), or introduce a [private code](user_guide/new_coding_schemes.md).
* MeasurementUnitsCode: Units of measurement. DICOM uses [Unified Code of Units of Measurement (UCUM)](http://unitsofmeasure.org/trac) code system (CodingSchemeDesignator `UCUM`) to describe units. Some of the commonly used unit codes are listed in [CID 7181](http://dicom.nema.org/medical/dicom/current/output/chtml/part16/sect_CID_7181.html), but as discussed in [Part 16 Section 7.2.2](http://dicom.nema.org/medical/dicom/current/output/chtml/part16/sect_7.2.2.html), any of the UCUM codes can be used in DICOM.
* DerivationCode: select code from [CID 7203](http://dicom.nema.org/medical/dicom/current/output/chtml/part16/sect_CID_7203.html), or introduce a [private code](user_guide/new_coding_schemes.md).
* AnatomicRegionSequence and Modifier (optional): follow the same guidelines as discussed for [Segmentation](#segmentation)
anatomic codes selection.
* MeasurementMethodCode (optional): code describing the model used for deriving the quantity.
* ModelFittingMethodCode (optional): code describing the model fitting method.

In the future we plan to provide specific recipes that describe the sets of codes suitable for specific use-cases (e.g., estimating Apparent Diffusion Coefficient (ADC) from Diffusion-Weighted MRI, or performing pharmacokinetic modeling of the Dynamic Contrast-Enhanced MRI). 

Relevant development of the codes related to ADC calculation can be found in DICOM Correction Proposal (CP)[CP-1665](ftp://medical.nema.org/medical/dicom/cp/cp1665_lb_ADCmodelparameters.pdf). These codes are expected to become part of the standard in Spring 2017.

For now, the best place to start is [this web application](http://qiicr.org/dcmqi/#/validators) (select `pm-schema` in the Validation schema selector) that you can use to choose an existing example and modify it to tailor to your use case.




