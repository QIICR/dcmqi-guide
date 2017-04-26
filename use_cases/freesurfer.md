# Multi-structure segmentation of the brain

## Summary of the use case

Segmentations of
* grey and white matter (labels 4 and 5 in [this NIfTI file](http://slicer.kitware.com/midas3/download/item/285812/gmwm-dcmspace.nii.gz))
* left and right cerebral hemispheres (labels 2 and 3 in [this NIfTI file](http://slicer.kitware.com/midas3/download/item/285813/lhrh-dcmspace.nii.gz))
* overall brain mask (label 1 in [this NIfTI file](http://slicer.kitware.com/midas3/download/item/285811/brainmask-dcmspace.nii.gz))

were produced by FreeSurfer by segmenting [this DICOM MR series](http://slicer.kitware.com/midas3/download/item/285806/CMET-MRhead.zip).

## Conversion to DICOM SEG

### Visualization of the input data

As a first step, it is always a good idea to visualize the input data. We recommend [3D Slicer](http://slicer.org) for visualizing both DICOM and research format data.

Import DICOM dataset into Slicer DICOM database by unzipping the downloaded file with the MR series, opening DICOM Browser, and importing the directory with the DICOM MR series files (choose "Add link" when prompted).

![](/use_cases/dicom_import.jpg)

Next, load segmentations stored in NIfTI. To do that, open "Add data", select the NIfTI segmentation files, check "Options" flag, and check "LabelMap" for each of the files in the list. This will tell Slicer that the files you are loading contain segmentations.

![](/use_cases/load_labels.jpg)

You should be able to see image with the segmentation for one of the labels shown in the overlay. If you mouse over the little pin icon in the corner of the slice viewer (orange arrow in the screenshot below), then expand the popup panel (red arrow), you can select each of the label files in the label selector (green arrow).

![](/use_cases/select_overlay.jpg)

To figure out the value for the individual labels in the overlay, you can mouse over the label, and then look in the Data Probe panel in the bottom left corner of the Slicer window (red arrow in the screenshot below).

![](/use_cases/data_probe.jpg)

Using the hints discussed above, we can figure out that:
* 1 = overall brain mask label in `brainmask-dcmspace.nii.gz`
* 2 = left hemisphere label in `lhrh-dcmspace.nii.gz`
* 3 = right hemisphere label in `lhrh-dcmspace.nii.gz`
* 4 = gray matter label in `gmwm-dcmspace.nii.gz`
* 5 = white matter label in `gmwm-dcmspace.nii.gz`

![](/use_cases/freesurfer-usecase-all_labels.jpg)

We will need the information above in the process of data conversion!

### Preparation of the metadata JSON

In order to do the conversion, we need to pass extra metadata that would describe the segmentations to the converter.

We can use the web application that accompanies `dcmqi` to populate the metadata JSON file: http://qiicr.org/dcmqi/#/seg.

We will need to create a new _segment_ item in the web application for each of the labels we identified in the steps above. Here is how it will look like for label 1 (brain mask). Note that the "Segmented Property Type" drop-down contains a lot of items - start typing the name of the structure you are looking for, and it will suggest the matching names for you!

![](/use_cases/fs-brain-webapp.jpg)

Next let's add new segments for left and right hemisphere, and gray and white matter in this specific order. Add new segment by pushing the "+" button in the web application interface:

![](/use_cases/add-segment-webapp.jpg)

Note that you will not find codes for brain cerebral hemispheres in the drop-down - select brain ventricle as a placeholder, and select modifiers to be Left and Right for segments 2 and 3. We will put the proper codes at a later stage.

After populating all segments, you should see the following metadata for each of the individual segments in the webapp:

|Segment|metadata as shown in the webapp|
|--|--|
|1|![](/use_cases/fs-segment1.jpg)|  
|2|![](/use_cases/fs-segment2.jpg)|
|3|![](/use_cases/fs-segment3.jpg)|
|4|![](/use_cases/fs-segment4.jpg)|
|5|![](/use_cases/fs-segment5.jpg)|

Next, click OK button in the webapp to generate the JSON metafile, and download it to your computer.

![](/use_cases/download_json.jpg)

It should look something like [this](https://gist.github.com/fedorov/34d1ccbd7d8a12f458a1d7c976c52e35#file-metadata-json).

### Cleanup of the JSON file

Some extra work is needed to finalize the JSON file you generated.

Open JSON file in your favorite text editor (we recommend [Atom](https://atom.io), a hackable text editor developed by GitHub). If you use Atom, we also recommend you install [pretty-json](https://atom.io/packages/pretty-json) extension, as it will help you validate your JSON file and make it more readable by adjusting indentation.

We will need to complete two tasks to finalize the JSON file.

**First**, we need to replace "Brain ventricle" and assign the proper code to the cerebral hemispheres. To find the code, open [DICOM part 16](http://dicom.nema.org/medical/dicom/current/output/html/part16.html) in your browser (be patient, this is a very large page!). Once loading is completed, search for the word "hemisphere" on that page. One of the hits leads us to table CID 12022, which contains the code we need!

![](/use_cases/hemisphere_code.jpg)

Replace all occurrences of "Brain ventricle" in your JSON file with "Cerebral hemisphere", and all codes "T-A1600" with "T-A010F". The result should look like [this](https://gist.github.com/fedorov/34d1ccbd7d8a12f458a1d7c976c52e35#file-metadata_codes_updated-json).

**Second**, we need to reshuffle the content of the file. The "segmentAttributes" attribute of the JSON file is a list of lists, where each of the inner lists corresponds to one input file passed to the converter and contains metainformation for each of the segments. In our case, we have 3 input files. In the JSON file produced by the webapp, we have only one inner list with all of the segments. Edit the JSON file to have the first list contain just the brain mask, second list contain left and right cerebral hemispheres, and the third list containing gray and white matter. The resulting file should look like [this](https://gist.github.com/fedorov/34d1ccbd7d8a12f458a1d7c976c52e35#file-metadata_three_inner_lists-json).

**Now we should be ready to run the converter!
**

### Run the converter

You can follow the [installation instructions](https://qiicr.gitbooks.io/dcmqi-guide/content/user_guide/installation.html) and use either binary package for your platform, or a Docker image.

Considering you have the MR series in a folder called `CMET-MRhead` (it will be in that folder if you downloaded the dataset using the link on top of this page), segmentations in the files as specified on top, and your metadata JSON in `meta.json`, you should be able to run this command line to do the conversion:

```
$ itkimage2segimage --inputDICOMDirectory CMET-MRhead \
   --inputMetadata meta.json \
   --inputImageList brainmask-dcmspace.nii.gz,lhrh-dcmspace.nii.gz,gmwm-dcmspace.nii.gz \
   --outputDICOM freesurfer_seg.dcm
```

Once the converter completes without errors, the resulting DICOM SEG object should be in `freesurfer_seg.dcm`!

### Load the result into 3D Slicer

As a prerequisite, [make sure you have `QuantitativeReporting` extension of 3D Slicer installed](https://qiicr.gitbooks.io/quantitativereporting-guide/content/)! Without this extension, 3D Slicer will not know how to interpret DICOM SEG data!

Once `QuantitativeReporting` is installed, import the directory with the `freesurfer_seg.dcm` file. You should then see a new series with modality `SEG` in the DICOM Browser:

![](/use_cases/seg_series.jpg)

Select this series and load it. Slicer will prompt you whether you want to load the MR series which is referenced from the segmentation object. 3D rendering of the segmentation surfaces will be shown automatically in 3D and as overlay in the slice viewers. Switch to Segmentations module of 3D Slicer to control visibility and opacity of individual segments, access information about the semantics of the segments, and perform other operations. 

![](/use_cases/fs-slicer-view.jpg)

**This is it!**

## Advantages offered by using DICOM SEG

Too many to count, of course ... TBD

## Potential issues

### Picking vocabulary terms is pain!

It just is ...

### Size of the DICOM SEG representation is much larger than input

```
$ ls -lat
total 81944
drwx------+ 329 fedorov  staff     11186 Apr 26 17:50 ..
drwxr-xr-x   11 fedorov  staff       374 Apr 26 17:43 .
-rw-r--r--    1 fedorov  staff    652400 Apr 26 17:55 freesurfer_seg.dcm.zip
-rw-r--r--    1 fedorov  staff  13061414 Apr 26 17:43 freesurfer_seg.dcm
-rw-r--r--@   1 fedorov  staff  27799607 Apr 26 17:35 CMET-MRhead.zip
-rw-r--r--@   1 fedorov  staff      3918 Apr 26 17:17 meta.json
-rw-r--r--@   1 fedorov  staff    331890 Apr 26 16:05 lhrh-dcmspace.nii.gz
-rw-r--r--@   1 fedorov  staff    514384 Apr 26 16:05 gmwm-dcmspace.nii.gz
-rw-r--r--@   1 fedorov  staff    226039 Apr 26 16:05 brainmask-dcmspace.nii.gz
```
