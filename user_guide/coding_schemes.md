# Codes, Coding schemes, Terminologies, etc...

One of the fundamental principles of DICOM is the use of _controlled terminologies_, or _lexicons_, or _coding schemes_ (for the purposes of this guide, these can be used interchangeably). While using `dcmqi` you will encounter various situations where you will need to select codes to describe the data you are converting into DICOM. In this and the following sections we give you an overview and describe the general principles of deciding on how to choose such codes.

Controlled terminologies define a set of codes, and sometimes their relationships, that are carefully curated to describe entities for a certain application domain. Consistent use of such terminologies helps with uniform data collection and is critical for harmonization of activities conducted by  independent groups.

When codes are used in DICOM, they are saved as triplets that consist of
* CodeValue: unique identifier for a term
* CodingSchemeDesignator: code for the authority that issued this code
* CodeMeaning: human-readable code description

DICOM relies on various sources of codes, all of which are listed in [PS3.16 Section 8](http://dicom.nema.org/medical/dicom/current/output/chtml/part16/chapter_8.html) of the standard.

The first question while choosing the coding scheme is whether you will use an existing code, or define your own. Depending on your choice, follow one of the two recipes below.

1. [**Use one of the codes defined in DICOM**](existing_coding_scheme.md): 

2. If there is no matching code that is already included in DICOM, you can [**search one of the existing terminologies/ontologies for a suitable code**](searching_codes_outside_dicom.md): this option often requires more effort, but it will make your resulting data consistent with the existing controlled terminologies, facilitating reasoning on the resulting data, and aggregation of the results collected from different sources.  This “semantic” approach using standard codes allows for greater reuse and harmonization with other data sets, since the need for natural language parsing of plain text during “data mining” is obviated by the commonality of standard codes for standard entities, such as anatomical regions, types of tumor, etc. The choice of the coding scheme and specific codes will depend on the specific data conversion task.

3. [**Introduce a new coding scheme**](new_coding_scheme.md): if you follow this option, you can either reuse an existing terminology which is not listed in the DICOM standard [here](http://dicom.nema.org/medical/dicom/current/output/chtml/part16/chapter_8.html), or define your own terminology and set of codes. You can still produce the data that is harmonized with other sources, but to achieve this you will need to make sure all "data producers" follow your coding scheme! This approach is suitable when you are working on an application where no established terminology exists (e.g., no consensus within your community is reached), or when semantic interoperability is not a priority.

