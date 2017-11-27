# Segmentations and measurements from prostate MRI

## Summary of the use case

In this use case we will summarize the approach to encode segmentations of various structures and measurements derived using those segmentations from multi-parametric Magnetic Resonance Imaging \(MRI\) of the prostate.

More specifically, we will discuss encoding of the imaging-derived data discussed in the following paper:

> Fedorov A, Vangel MG, Tempany CM, Fennessy FM. Multiparametric Magnetic Resonance Imaging of the Prostate: Repeatability of Volume and Apparent Diffusion Coefficient Quantification. _Investigative Radiology_; 2017;52\(9\):538–546 [http://dx.doi.org/10.1097/RLI.0000000000000382](http://dx.doi.org/10.1097/RLI.0000000000000382).

The imaging-derived data discussed in that paper consists of the two components:

1. Segmentations of the following structures:

   * Whole gland of the prostate
   * Peripheral zone of the prostate
   * Suspected tumor area of the prostate within the peripheral zone of the prostate
   * Normal-appearing area of the peripheral zone

2. Segmentation-based measurements: the manuscript is concerned with evaluating the repeatability of the

   * mean volume of the regions listed above, and
   * mean Apparent Diffusion Coefficient \(ADC\) values calculated over the segmentation-defined regions.

Each segmented structure is saved in an individual itk nrrd format file.

## Conversion of Segmentations to DICOM SEG

### Organizing the Data

It is advisable to organize the original dicom image files in a directory structure like `<Patient>/<Study>/<Series>/orig-img-dicom/`. Since one required input of the converter are all dicom files which represent the original image on which the segmentation was created. Organizing the data as described above will allow us to just provide the correct series directory instead of listing all files of that series.

All segmentations corresponding to a series should then be put into a folder like `<Patient>/<Study>/<Series>/segmentations/`. Into this folder we will also put the meta-information JSON file required to convert the segmentations.

### Creating the Meta-Information JSON File

The first convenient place to start with generating JSON files for the `dcmqi` SEG converter is always [http://qiicr.org/dcmqi/\#/seg](http://qiicr.org/dcmqi/#/seg).

The most confusing part is typically how to find the codes to encode specific items. In our case, some structures have codes already included in the DICOM standard, while some other codes had to be looked up in SNOMED.

Make sure to select DICOM master list in the "Segmentation Category Type Context Name". This will give you access to all of the codes in the standard.

* Whole prostate gland: we encode anatomic structure \(short selection list in the Category selector\), and can quickly locate the code for the prostate:
  ```
        "SegmentedPropertyCategoryCodeSequence": {
          "CodeValue": "T-D000A",
          "CodingSchemeDesignator": "SRT",
          "CodeMeaning": "Anatomical Structure"
        },
        "SegmentedPropertyTypeCodeSequence": {
          "CodeValue": "T-9200B",
          "CodingSchemeDesignator": "SRT",
          "CodeMeaning": "Prostate"
        }
  ```
* Peripheral zone: this one is more difficult - no code could be located. We needed to consult [http://browser.ihtsdotools.org/](http://browser.ihtsdotools.org/) to find "Peripheral zone of the prostate", which has SCTID 279706003. Next, we looked up SRT code for SCTID in an older version of SNOMED \(this was done by our good friend David Clunie!\). Result:
  ```
        "SegmentedPropertyCategoryCodeSequence": {
          "CodeValue": "T-D000A",
          "CodingSchemeDesignator": "SRT",
          "CodeMeaning": "Anatomical Structure"
        },
        "SegmentedPropertyTypeCodeSequence": {
          "CodeValue": "T-D05E4",
          "CodingSchemeDesignator": "SRT",
          "CodeMeaning": "Peripheral zone of the prostate"
        }
  ```
* Suspected tumor tissue: the most suitable code is "Lesion", but it is not available in the web interface we used before ... We will need to fix this... Note that for "Morphologically altered structure" we can \(and should!\) also specify "AnatomicRegionSequence" to encode the location of the lesion. In our case, all of the lesions are in the peripheral zone of the prostate.
  ```
        "SegmentedPropertyCategoryCodeSequence": {
          "CodeValue": "M-01000",
          "CodingSchemeDesignator": "SRT",
          "CodeMeaning": "Morphologically Altered Structure"
        },
        "SegmentedPropertyTypeCodeSequence": {
          "CodeValue": "M-01100",
          "CodingSchemeDesignator": "SRT",
          "CodeMeaning": "Lesion"
        },
        "AnatomicRegionSequence": {
          "CodeValue": "T-D05E4",
          "CodingSchemeDesignator": "SRT",
          "CodeMeaning": "Peripheral zone of the prostate"
        }
  ```
* Normal tissue: here we can use generic "Tissue" for category, and a special code \(which again is not available in our web interface\) for Normal tissue:
  ```
        "SegmentedPropertyCategoryCodeSequence": {
          "CodeValue": "T-D0050",
          "CodingSchemeDesignator": "SRT",
          "CodeMeaning": "Tissue"
        },
        "SegmentedPropertyTypeCodeSequence": {
          "CodeValue": "G-A460",
          "CodingSchemeDesignator": "SRT",
          "CodeMeaning": "Normal"
        },
        "AnatomicRegionSequence": {
          "CodeValue": "T-D05E4",
          "CodingSchemeDesignator": "SRT",
          "CodeMeaning": "Peripheral zone of the prostate"
        }
  ```

Other attributes are rather trivial to populate:

* `BodyPartExamined` should be `PROSTATE` \(all caps is important! ... we should add explanation to this ...\)
* `SegmentDescription` can be populated with an abbreviated name reflecting the structure being segmented \(note: this attribute can be at most 64 characters long!\)
* `SegmentAlgorithmName` should be set to `Slicer`
* `SegmentAlgorithmType` to `MANUAL`, since that is how segmentations were created
* Colors: these are up to the creator, but it is usually a good idea to pick colors that allow to easily differentiate regions. We use the color assignment summarized in [this spreadsheet](https://docs.google.com/spreadsheets/d/1A9N0wzag1GlVkjbck8XYV_kCqKPbcxJbua-e2Arv2V0/edit?usp=sharing).

An almost complete meta-information JSON file for these cases could look like this:

```
{
  "@schema": "https://raw.githubusercontent.com/qiicr/dcmqi/master/doc/schemas/seg-schema.json#",

  "ContentCreatorName": "Reader01",
  "ClinicalTrialSeriesID": "Session01",
  "ClinicalTrialTimePointID": "@TimePoint@",
  "SeriesDescription": "Segmentation",
  "SeriesNumber": "@SeriesNumber@",
  "InstanceNumber": "1",
  "BodyPartExamined": "PROSTATE",
  "segmentAttributes": [
    [
     {
        "labelID": 1,
        "SegmentDescription": "WholeGland",
        "SegmentAlgorithmType": "MANUAL",
        "SegmentAlgorithmName": "Slicer",
        "SegmentedPropertyCategoryCodeSequence": {
          "CodeValue": "T-D000A",
          "CodingSchemeDesignator": "SRT",
          "CodeMeaning": "Anatomical Structure"
        },
        "SegmentedPropertyTypeCodeSequence": {
          "CodeValue": "T-9200B",
          "CodingSchemeDesignator": "SRT",
          "CodeMeaning": "Prostate"
        },
        "recommendedDisplayRGBValue": [51, 204, 0]
      }
    ],
    [
      {
        "labelID": 2,
        "SegmentDescription": "PeripheralZone",
        "SegmentAlgorithmType": "MANUAL",
        "SegmentAlgorithmName": "Slicer",
        "SegmentedPropertyCategoryCodeSequence": {
          "CodeValue": "T-D000A",
          "CodingSchemeDesignator": "SRT",
          "CodeMeaning": "Anatomical Structure"
        },
        "SegmentedPropertyTypeCodeSequence": {
          "CodeValue": "T-D05E4",
          "CodingSchemeDesignator": "SRT",
          "CodeMeaning": "Peripheral zone of the prostate"
        },
        "recommendedDisplayRGBValue": [153, 90, 50]
      }
    ],
    [
      {
        "labelID": 3,
        "SegmentDescription": "TumorROI_PZ_1",
        "SegmentAlgorithmType": "MANUAL",
        "SegmentAlgorithmName": "Slicer",
        "SegmentedPropertyCategoryCodeSequence": {
          "CodeValue": "M-01000",
          "CodingSchemeDesignator": "SRT",
          "CodeMeaning": "Morphologically Altered Structure"
        },
        "SegmentedPropertyTypeCodeSequence": {
          "CodeValue": "M-01100",
          "CodingSchemeDesignator": "SRT",
          "CodeMeaning": "Lesion"
        },
        "AnatomicRegionSequence": {
          "CodeValue": "T-D05E4",
          "CodingSchemeDesignator": "SRT",
          "CodeMeaning": "Peripheral zone of the prostate"
        },
        "recommendedDisplayRGBValue": [255, 255, 0]
      }
    ],
    [
       {
        "labelID": 8,
        "SegmentDescription": "NormalROI_PZ_1",
        "SegmentAlgorithmType": "MANUAL",
        "SegmentAlgorithmName": "Slicer",
        "SegmentedPropertyCategoryCodeSequence": {
          "CodeValue": "T-D0050",
          "CodingSchemeDesignator": "SRT",
          "CodeMeaning": "Tissue"
        },
        "SegmentedPropertyTypeCodeSequence": {
          "CodeValue": "G-A460",
          "CodingSchemeDesignator": "SRT",
          "CodeMeaning": "Normal"
        },
        "AnatomicRegionSequence": {
          "CodeValue": "T-D05E4",
          "CodingSchemeDesignator": "SRT",
          "CodeMeaning": "Peripheral zone of the prostate"
        },
        "recommendedDisplayRGBValue": [51, 204, 0]
      }
    ]
  ],
  "ContentLabel": "SEGMENTATION",
  "ContentDescription": "Image segmentation",
  "ClinicalTrialCoordinatingCenterName": "dcmqi"
}
```

Note that this file contains two placeholders which still need to be replaced with the correct value for the segmentations we are trying to convert: `@TimePoint@` and `@SeriesNumber@` . While all other properties in the JSON file are valid for all segmentation files, these two properties will differ for different segmentation files:

1. In this particular case we have segmentations of two timepoints, so we have to make sure the meta-information JSON file we use when running the converter has the correct timpoint encoded.
2. Segmentations based on one series in a study should also have a unique series number within that study. We can follow the following formula \(or similar\) to assign series number: `<SeriesNumber of the image series>+1000`.

This means we will need several slightly different JSON files to perform the conversion. The best approach is to create them dynamically with a script that inserts the correct values for the placeholders. If we follow the data organization approach suggested above we should have one JSON file per `<Patient>/<Study>/<Series>/segmentations/` folder.

### Running the Converter

The converter needs to run separately for each segmentation folder. Switch to a `<Patient>/<Study>/<Series>/segmentations/` folder. Then run the converter like this:

```
itkimage2segimage.exe
--inputImageList WholeGland.nrrd,PeripheralZone.nrrd,TumorROI_PZ_1.nrrd,NormalROI_PZ_1.nrrd
--inputDICOMDirectory <Patient>/<Study>/<Series>/orig-img-dicom/
--inputMetadata meta.json
--outputDICOM <out-name>.SEG.dcm
```

This will place a file `<out-name>.SEG.dcm` into the segmentations folder. The SEG object will contain all four segmentations.

Note that the order of the files for `--inputImageList` has to exactly match the order of the `segmentAttributes` list in the meta.json. If the order of files in `--inputImageList` is different or contains less or more files, we need to adjust the meta.json accordingly.

## Conversion of measurements to DICOM SR TID1500

Overall, the conversion of segmentation-based measurements into a DICOM Structured Report that follows [SR template 1500](http://dicom.nema.org/medical/dicom/current/output/chtml/part16/chapter_A.html#sect_TID_1500) is supported by the `dcmqi` tool `tid1500writer`. This tool expects as input the following items:

* DICOM image series that was used for segmentation
* DICOM Segmentation image series containing the segmentation result
* JSON file containing the individual measurements, and additional metadata needed by DICOM

Assuming segmentation conversion above was successful, a JSON file that specifies metadata for the structured report overall, and for the individual measurements.

We do not provide a web application to populate such file, so at this moment, the easiest is to start with an example and update it as needed. Let's start with [this sample dataset](https://github.com/QIICR/dcmqi/blob/master/doc/examples/sr-tid1500-ct-liver-example.json).

Before discussing how to initialize individual items, we need to decide how to organize measurements. There are at least two options here:  
1. Save measurements for each combination of structure/image series as a separate DICOM SR document.  
2. Save measurements for all structures segmented in a given series in a single DICOM SR document.

Considering we made a decision to save all segments \(whole gland, peripheral zone, tumor region, etc\) in a single DICOM SEG file, it is logical to follow the same pattern and store per-segment measurements in separate groups within the same DICOM SR.

Here are the items we will need to update:

---

```
"SeriesDescription": "Measurements",
"SeriesNumber": "1001",
"InstanceNumber": "1",
```

Although `SeriesNumber` is not formalized by DICOM, it is usually expected by the users that it should be unique. We can follow the following formula \(or similar\) to assign series number: `<SeriesNumber of the image series being segmented>+2000`.

---

```
"compositeContext": [
  ...
]
```

This item contains the name of a DICOM file that should be used to populate composite context \(information about study, patient, equipment, which is the same for all series in a study\) of the output DICOM object. We can set this item to be the Segmentation DICOM object, or any of the DICOM instances from the input image.

---

```
"imageLibrary": [
  ...
],
```

`imageLibrary` should contain the list of all DICOM instances from the source image series being segmented. A component of the output SR document will include certain attributes of these instances, and will reference them by `SOPInstanceUID`.

---

```
"observerContext": {
  "ObserverType": "PERSON",
  "PersonObserverName": "Reader1"
},
```

In our case, there was a single reader, so we can leave this item unchanged.

---

```
"VerificationFlag": "VERIFIED",
"CompletionFlag": "COMPLETE",

"activitySession": "1",
"timePoint": "1",
```

These items can also remain unchanged, since we share final measurements. The exception is `timePoint`, which should have values of 1 for baseline, and 2 for the followup.

---

Next, we need to populate the list of `Measurements`. Each of the items \(measurement groups\) in this list will contain a list of attributes that apply to all individual measurements within the group, and a list of individual measurements.

In our case, each measurement group will contain measurements calculated over a single segmented structure.

First, let's look at the top-level attributes:

```
  "Measurements": [
    {
      "TrackingIdentifier": "Measurements group 1",
```

`TrackingIdentifier` is a human-readable description of the measurements group. We can use the pattern `<structure name> measurements`, i.e., `Whole gland measurements` etc.

---

```
      "ReferencedSegment": 1,
      "SourceSeriesForImageSegmentation": "1.2.392.200103.20080913.113635.1.2009.6.22.21.43.10.23430.1",
      "segmentationSOPInstanceUID": "1.2.276.0.7230010.3.1.4.0.42154.1458337731.665796",
```

These items should be propagated given the information about source image series and the segmentation generated in the previous conversion step. Availability of these items allows to link measurements with the results of segmentation and source image series.

---

The next items -- `Finding` and `FindingSite` -- are code tuples that allows us to encode what was the region over which measurement was done, and where it was located. These items are somewhat similar to what we had to specify for encoding segmentation.

Here are the codes we can use for each of the structures over which we performed the measurements \(each tuple in parentheses contains `(CodeMeaning, CodingSchemeDesignator, CodeValue)`:

* Whole gland: 
  * Finding: `("Entire Gland", "SRT", T-F6078)`
  * Finding site: `("Prostate", "SRT", "T-9200B")`
* Peripheral zone:
  * Finding: `("Entire", "SRT", "R-404A4")`
  * Finding site: `("Peripheral zone of the prostate", "SRT", "T-D05E4")`
* Suspected tumor tissue:
  * Finding: `("Abnormal", "SRT", "R-42037")`
  * Finding site: `("Peripheral zone of the prostate", "SRT", "T-D05E4")`
* Normal tissue:
  * Finding: `("Normal", "SRT", "G-A460")`
  * Finding site: Finding site: `("Peripheral zone of the prostate", "SRT", "T-D05E4")`

---

Following the description of the top-level attributes for a measurement group is the list of individual measurements. Each measurement item must include the following attributes:

* `value`: the measurement value
* `quantity`, `units`, and `derivationModifier`: coded tuples describing the quantity. In our case, measurements are either volume of the segmented regions, or the mean value of the Apparent Diffusion Coefficient \(ADC\).

**Volume**:

```
"quantity": {
  "CodeValue": "G-D705", 
  "CodingSchemeDesignator": "SRT", 
  "CodeMeaning": "Volume")
 },
"units": {
  "CodeValue": "cm3",
  "CodingSchemeDesignator": "UCUM",
  "CodeMeaning": "cubic centimeter"
}
```

**Mean ADC** \(note how the fact that we encode the mean value of ADC over the region is post-coordinated with the `derivationModifier`\):

```
"quantity": {    
  "CodeValue": "113041",
  "CodingSchemeDesignator": "DCM",
  "CodeMeaning": "Apparent Diffusion Coefficient"
},
"units": {
   "CodeValue": "um2/s",
   "CodingSchemeDesignator": "UCUM",
   "CodeMeaning": "um2/s"
 },
"derivationModifier": {
   "CodeValue": "R-00317",
   "CodingSchemeDesignator": "SRT",
   "CodeMeaning": "Mean"
}
```

## Conversion of the ADC maps

Optionally, we can also encode the ADC maps generated on the GE imaging post-processing equipment and stored as MR objects. Instead, we can use the DICOM Parametric map object, since it allows to explicitly communicate quantity, units and the type of ADC fitting approach that was used. [This example JSON](https://github.com/QIICR/dcmqi/blob/master/doc/examples/pm-example.json) can be used directly for this conversion task.

