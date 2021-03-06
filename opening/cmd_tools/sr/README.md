# Measurements

`dcmqi` provides command line tools to convert lists of measurements calculated from the images for the regions defined by rasterized segmentations into DICOM representation. Specifically, the DICOM representation suitable for such data is DICOM Structured Reporting \(SR\) [Template ID 1500](http://dicom.nema.org/medical/dicom/current/output/chtml/part16/chapter_A.html#sect_TID_1500).

Each measurement is associated with a specific segment in the corresponding DICOM Segmentation object. For each measurements, Quantity, Units and Derivation \(when appropriate\) must be specified as coded tuples. Multiple measurements can be assigned in a list for the individual segment.

At the moment, the measurements must be specified in a JSON file, such as the one shown in [this example](https://github.com/QIICR/dcmqi/blob/master/doc/examples/sr-tid1500-ct-liver-example.json). [It is in our plans](https://github.com/QIICR/dcmqi/issues/165) to provide support of the CSV format for bi-directional conversion of the measurements data.

