# Update Appveyor build dependencies

`dcmqi` is using pre-built binaries of ITK and DCMTK for Appveyor to reduce the overall build time of the library.

Every time the release of DCMTK/ITK needs to be updated, it is important to re-generate these binaries, otherwise Appveyor build will fail when there are API changes.

## Updating DCMTK and ITK packages

The modified version of DCMTK is located in the dcmqi branch of the [dcmtk-dcmqi repository](https://github.com/QIICR/dcmtk-dcmqi). The only difference from the stock DCMTK source code is the `appveyor.yml` file. Thus, the steps are the following:

1. Check out the version of DCMTK that needs to be used
2. Rebase the dcmqi branch of dcmtk-dcmqi to the new version.
3. Update the name of the package to reflect the version and date of the DCMTK package to be created \(`deploy/release` section of the `appveyor.yml`\)
4. Push the updated dcmqi branch to `QIICR/dcmtk-dcmqi`

Appveyor build will be triggered automatically, and barring any build issues a new release will be uploaded to [https://github.com/QIICR/dcmtk-dcmqi/releases](https://github.com/QIICR/dcmtk-dcmqi/releases).

The URL corresponding to DCMTK-dcmqi.zip in the release should then be used to first update [https://github.com/QIICR/ITK-dcmqi/blob/dcmqi/appveyor.yml](https://github.com/QIICR/ITK-dcmqi/blob/dcmqi/appveyor.yml), which should result in re-generated ITK-dcmqi package under ITK-dcmqi releases in [https://github.com/QIICR/ITK-dcmqi/releases](https://github.com/QIICR/ITK-dcmqi/releases). At that time, URLs for both ITK-dcmqi and DCMTK-dcmqi packages should be updated in the `dcmqi` appveyor script file: [https://github.com/QIICR/dcmqi/blob/master/appveyor.yml](https://github.com/QIICR/dcmqi/blob/master/appveyor.yml).

