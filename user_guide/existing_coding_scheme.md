# Using DICOM defined coding schemes

In DICOM, the process of choosing a code, and a coding scheme, depends on the context. In the following, we will discuss some of the guidelines that can be used to choose suitable codes for the tasks of segmentation, measurements and parametric map conversion supported by `dcmqi`.

* [Segmentation](user_guide/existing_coding_scheme.md#Segmentation)
* [Measurements](user_guide/existing_coding_scheme.md#Measurements)
* [Parametric map](user_guide/existing_coding_scheme.md#Parametric map)

## Segmentation

While converting segmentations, you will need to define the following coded entities in the JSON file:

* SegmentedPropertyCategoryCodeSequence
* SegmentedPropertyTypeCodeSequence
* SegmentedPropertyTypeModifierCodeSequence (when applicable)
* AnatomicRegionSequence (when applicable)
* AnatomicRegionModifierSequence (when applicable)

This looks overwhelming indeed! That's why we developed [a web application](http://qiicr.org/dcmqi/#/seg) to help you interactively choose the codes for each of those items (you can also use this interactive application 

For each of these attributes, DICOM provides guidance on the selection of the suitable codes.

* SegmentedPropertyCategoryCodeSequence code choices are defined in ...

**TBD**

When describing anatomic entities, [Systematized Nomenclature of Medicine Clinical Terms (SNOMED® CT)](https://en.wikipedia.org/wiki/SNOMED_CT) coding scheme (DICOM CodingSchemeDesignator `SRT`) is the preferred controlled terminology. Most, if not all, of the codes used to define the entities above are from SNOMED CT. SNOMED CT maintains a systematically organized computer processable collection of medical terms providing codes, terms, synonyms and definitions used in clinical documentation and reporting. SNOMED CT maintains the hierarchy of relationships among the codes, which can be used for semantic reasoning on the data. As an example, see [hierarchy of the entities related to the Liver code in the SNOMED CT Browser](http://browser.ihtsdotools.org/?perspective=full&conceptId1=181268008&edition=en-edition&release=v20160731&server=http://browser.ihtsdotools.org/api/snomed&langRefset=900000000000509007).

### SNOMED CT License exemption

Note that SNOMED CT codes included in the DICOM standard are exempt from [SNOMED CT license](http://www.snomed.org/snomed-ct/get-snomed-ct). The details are discussed in [this blog post](http://dclunie.blogspot.com/2016/03/dicom-and-snomed-back-in-bed-together.html):

> Users and commercial and open source DICOM developers can be reassured that they may continue to use the subset of SNOMED concepts in the DICOM standard in their products and software, globally and without a fee or individual license.

## Measurements

## Parametric map