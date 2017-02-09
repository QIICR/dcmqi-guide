# Codes, Coding schemes, Terminologies, etc...

One of the fundamental principles of DICOM is the use of _controlled terminologies_, or _lexicons_, or _coding schemes_ (for the purposes of this guide, these can be used interchangeably). While using `dcmqi` you will encounter various situations where you will need to select codes to describe the data you are converting into DICOM. In this and the following sections we give you an overview and describe the general principles of deciding on how to choose such codes.

Controlled terminologies define a set of codes, and sometimes their relationships, that are carefully curated to describe entities for a certain application domain. Consistent use of such terminologies helps with uniform data collection and is critical for harmonization of activities conducted by  independent groups.

When codes are used in DICOM, they are saved as triplets that consist of
* CodeValue: unique identifier for a term
* CodingSchemeDesignator: code for the authority that issued this code
* CodeMeaning: human-readable code description

DICOM relies on various sources of codes, all of which are listed in [PS3.16 Section 8](http://dicom.nema.org/medical/dicom/current/output/chtml/part16/chapter_8.html) of the standard.

The first question while choosing the coding scheme is whether you will use an existing code, or define your own. Depending on your choice, follow one of the two recipes below.

1. **Use an existing code**: this option often requires more effort, but it will make your resulting data harmonized with the existing controlled terminologies, facilitating reasoning on the resulting data an aggregation of the results collected from different sources.  
2. **Define a new code**: if you follow this option, you will define your own terminology and set of codes. You can still produce the data that is harmonized with other sources, but to achieve this you will need to make sure all "data producers" follow your coding scheme. 

In DICOM, the process of choosing a code, and a coding scheme, depends on the context. When describing anatomic entities, [Systematized Nomenclature of Medicine Clinical Terms (SNOMED® CT)](https://en.wikipedia.org/wiki/SNOMED_CT) coding scheme (DICOM CodingSchemeDesignator `SRT`) is the one example of a controlled terminology that maintains a systematically organized computer processable collection of medical terms providing codes, terms, synonyms and definitions used in clinical documentation and reporting. 