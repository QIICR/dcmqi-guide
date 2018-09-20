# Github release generation

To generate a release, you will need to set up GPG keys on your platform to sign the commits. You can follow [these GitHub instructions](https://help.github.com/articles/adding-a-new-gpg-key-to-your-github-account/) to complete that step.

1. Check that tests pass on all platforms
2. Edit `CMakeLists.txt` and update `DCMQI_VERSION_*` variables, update `README.md` file to point to the updated version number for the Docker image.
3. Commit changes using message like `cmake: Set DCMQI version to 1.0.7`
4. Create corresponding tag:

   ```text
   git tag -s -m "vX.Y.Z" vX.Y.Z master
   ```

5. Push tag and master \(in that order\) to trigger the release build and upload

   ```text
   git push origin vX.Y.Z
   git push origin master
   ```

6. Once new packages are generated, update documentation [Quick Start](https://qiicr.gitbooks.io/dcmqi-guide/content/quick-start.html) section to point to the new package.

## Under the hood ...

`dcmqi` is using `publish_github_release` from [scikit-ci-addons](http://scikit-ci-addons.readthedocs.io/) to upload and manage releases from CI to GitHub.

[This section](http://scikit-ci-addons.readthedocs.io/en/latest/addons.html#testing) of the `scikit-ci-addons` documentation describes how to troubleshoot issues related to this mechanism.

