# Quick Start

## Encoding analysis results in DICOM

Many research studies such as imaging clinical trials or retrospective analysis of clinical data use a collection of databases, spreadsheets, and research data file formats such as NRRD, NIfTI, etc. As an example, a common practice to share segmentations is to provide NIfTI files along with a CSV file mapping label numbers to anatomical names.  

As an alternative, you can use the `itkimage2segimage` command and related tools in `dcmqi` along with a JSON parameter file so that the segmentation output is described in terms of standardized vocabularies such as [SNOMED](https://en.wikipedia.org/wiki/Systematized_Nomenclature_of_Medicine), and segmentation can be saved in DICOM format side by side and cross-referenced with the source image data.  This can help remove ambiguity about the meaning of the results

Example command line:

```
itkimage2segimage --inputImageList brain-label.nrrd \
  --inputDICOMList brain1.dcm,brain2.dcm,brain3.dcm \
  --outputDICOM brain.SEG.dcm \
  --inputMetadata brain-label-mapping.json
```

## Using DICOM data with research applications

If you get quantitative imaging data in DICOM format but want to use it in MATLAB or ITK, you can use `segimage2itkimage` to extract conventional NIfTI files of the segmentations or `tid1500reader` to convert structured reports into JSON.

Example command line:

```
tid1500reader --inputDICOM pet-measurements.SR-1500.dcm \
  --outputMetadata pet-measurements.json
```

## Integrating DICOM into your analysis workflow

If you are designing a new analysis workflow from scratch, you can consider using DICOM as the format to store intermediate results.  If you look at the [Quantitative Reporting extension to 3D Slicer](https://www.slicer.org/wiki/Documentation/Nightly/Extensions/QuantitativeReporting), you can see how to create analysis results in native DICOM format that use standard terminologies and units, while retaining their linkage to the original input data.  You could consider using `dcmqi` utilities to convert to and from research formats.

