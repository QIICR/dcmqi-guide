# Limitations

These are some of the known limitations of `dcmqi`:

* It is currently not possible to use the converters when you don't have the source DICOM data; in the future, we are planning to add a mode of operation when all of necessary the metadata can be specified in the input JSON file.
* We do not support conversion of the data (segmentations, parametric maps or measurements) derived from enhanced multiframe images.
* Support of DICOM SR TID 1500 is limited to the measurements derived from the rasterized segmentations; we do not support all of the capabilities of TID 1500.