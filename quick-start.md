# Quick Start

To start using `dcmqi`, you can download the binary package with the command line converters for your operatings system following the links below for the latest \(v1.0.9\) release:

* [`dcmqi` **Windows package**](https://github.com/QIICR/dcmqi/releases/download/v1.0.9/dcmqi-1.0.9-win64.zip)
* [`dcmqi` **Linux package**](https://github.com/QIICR/dcmqi/releases/download/v1.0.9/dcmqi-1.0.9-linux.tar.gz)
* [`dcmqi` **macOS package**](https://github.com/QIICR/dcmqi/releases/download/v1.0.9/dcmqi-1.0.9-mac.tar.gz)

You can also find the packages corresponding to the latest version of the source code [here](https://github.com/QIICR/dcmqi/releases/tag/latest).

If you prefer using Docker, download the [`dcmqi` **image from DockerHub**](https://hub.docker.com/r/qiicr/dcmqi/) with `docker pull qiicr/dcmqi`.

If you want to build `dcmqi` yourself, or modify the source code - [get it on GitHub](https://github.com/qiicr/dcmqi).

## Encoding analysis results in DICOM

Many research studies such as imaging clinical trials or retrospective analysis of clinical data use a collection of databases, spreadsheets, and research data file formats such as NRRD, NIfTI, etc. As an example, a common practice to share segmentations is to provide NIfTI files along with a CSV file mapping label numbers to anatomical names.

As an alternative, you can use the `itkimage2segimage` command and related tools in `dcmqi` along with a JSON parameter file so that the segmentation output is described in terms of standardized vocabularies such as [SNOMED](https://en.wikipedia.org/wiki/Systematized_Nomenclature_of_Medicine), and segmentation can be saved in DICOM format side by side and cross-referenced with the source image data. This can help remove ambiguity about the meaning of the results

Example command line:

```text
itkimage2segimage --inputImageList brain-label.nrrd \
  --inputDICOMDirectory Brain-DICOMs \
  --outputDICOM brain.SEG.dcm \
  --inputMetadata brain-label-mapping.json
```

**Note for the Windows users**

We recommend you use _Windows PowerShell_ which is integrated in Windows 10 and further versions. The command line format on Windows will be different from that on Mac or Linux. Here is an example of the command line format on Windows \(applied to using `dcmqi` from Docker\):

```text
docker run -v C:\Users\joe\myWorkDirectory:/tmp/myWorkDirectory qiicr/dcmqi \
  itkimage2segimage --inputImageList /tmp/myWorkDirectory/brain-label.nrrd \
  --inputDICOMDirectory /tmp/myWorkDirectory/Brain-DICOMs \
  --outputDICOM /tmp/myWorkDirectory/brain.SEG.dcm \
  --inputMetadata /tmp/myWorkDirectory/brain-label-mapping.json
```

## Using DICOM data with research applications

If you get quantitative imaging data in DICOM format but want to use it in MATLAB or ITK, you can use `segimage2itkimage` to extract conventional NIfTI files of the segmentations or `tid1500reader` to convert structured reports into JSON.

Example command line:

```text
tid1500reader --inputDICOM pet-measurements.SR-1500.dcm \
  --outputMetadata pet-measurements.json
```

## Integrating DICOM into your analysis workflow

If you are designing a new analysis workflow from scratch, you can consider using DICOM as the format to store intermediate results. If you look at the [Quantitative Reporting extension to 3D Slicer](https://www.slicer.org/wiki/Documentation/Nightly/Extensions/QuantitativeReporting), you can see how to create analysis results in native DICOM format that use standard terminologies and units, while retaining their linkage to the original input data. You could consider using `dcmqi` utilities to convert to and from research formats.

