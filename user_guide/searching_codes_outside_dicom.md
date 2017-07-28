# Searching for codes outside DICOM

To find a suitable code in an existing terminology, you will need to know **what terminology to search**, and you will need **a tool that would facilitate your search**.

DICOM has a preference for using SNOMED-CT codes. If you find a code in SNOMED-CT that would fill an important gap, but is not in the standard, you can propose inclusion of that new code into the standard. However, keep in mind that:

1. the process of contributing something into the DICOM standard is lengthy and can take a year before you see your change in the standard text;
2. you will need to learn the procedures of contributing changes to the standard;
3. although the SNOMED-CT codes included in the standard are [exempt from the licensing constraints of SNOMED-CT](https://qiicr.gitbooks.io/dcmqi-guide/user_guide/existing_coding_scheme.html#snomed-ct-license-exemption), the ontology defined by SNOMED-CT is NOT covered by the standard. Therefore, you will still need a secondary ontology if you are concerned about the license, and want to do reasoning on your data.

Therefore, it can be more practical to find a suitable term in an ontology other than SNOMED-CT.

In this regard, David Clunie, the long-time Editor of the DICOM standard, gave the following guidance on what terminologies to consider when a gap in the standard is identified:

> [...] we (DICOM) use FMA then NeuroNames as a fallback when
there are no appropriate SNOMED codes (yet), and have contacts
with each of the appropriate groups to extend the schemes
as necessary. We have not used RadLex for anatomy, since it
is all/mostly(?) in FMA anyway (and if I recall correctly,
was derived from it, since the original RadLex protagonists
had no interest in reinventing that wheel).

> If you need to automate anatomical code mapping, consider
using the UMLS as a tool ... frequently both SNOMED and FMA
terms map to a common UMLS code which helps a lot.

>You can also use the FMAIDs included in the RadLex
ontology (http://purl.bioontology.org/ontology/RADLEX)
to map from RadLex back to FMA (or the reverse, since I
think the FMA OWL file also includes the RadLex RIDs),
then to UMLS and on to SNOMED, and indeed then undo the
pre-coordinated laterality (if necessary) using the SNOMED
hierarchy.

To search existing terminologies, you can consider using the following tools that search across different ontologies:

* [BioPortal](https://bioportal.bioontology.org/)
* [EMBL-EBI Ontology Lookup Service (OLS)](http://www.ebi.ac.uk/ols/index)

With both of these search engines you have an option of the advanced search to restrict the terms to a specific ontology.

There are also some search tools that provide searches for the individual ontologies (such as FMA), but in some instances their search capabilities are not flexible enough, and as such we recommend the aggregator BioPortal and OLS.

Here is an illustrative example of searching for a term "anterior cingulate gyrus", which is not included in DICOM (note that you can use `AnatomicRegionModifier` to encode laterality of the structure).

1. **Using BioPortal**, go to the [Advanced search](https://bioportal.bioontology.org/search?opt=advanced), put the search term in the search box, and specify FMA in the "Ontologies" selector:

![](/user_guide/assets/bioportal_search.jpg)

The search is successful, leading to [this entry](https://bioportal.bioontology.org/ontologies/FMA?p=classes&conceptid=http%3A%2F%2Fpurl.org%2Fsig%2Font%2Ffma%2Ffma61916), which includes the FMA ID 61916. Coding scheme designator for FMA is `FMA`, so the you can use the following code to describe the item:

```(61916, FMA, "Anterior cingulate gyrus")```

2. **Using OLS**, you can [select the specific ontology](http://www.ebi.ac.uk/ols/ontologies), and [search specifically in FMA](http://www.ebi.ac.uk/ols/ontologies/fma) for the same term, which will result in the same code:

![](/user_guide/assets/ols_search_result.jpg)

Note that OLS is (of writing this) using a "slimmed down version of FMA", but for common purposes perhaps this should still be sufficient.




