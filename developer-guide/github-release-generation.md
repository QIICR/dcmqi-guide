# `dcmqi` release generation on GitHub

To generate a release, you will need to set up GPG keys on your platform to sign the commits. You can follow [these GitHub instructions](https://help.github.com/articles/adding-a-new-gpg-key-to-your-github-account/) to complete that step.

1. Check that tests pass on all platforms

2. Edit `CMakeLists.txt` and update `DCMQI_VERSION_*` variables

3. Commit changes using message like `cmake: Set DCMQI version to 1.0.7`

4. Create corresponding tag:

  ```
  git tag -s -m "vX.Y.Z" vX.Y.Z master
  ```

5. Push tag and master (in that order) to trigger the release build and upload

  ```
  git push origin vX.Y.Z
  git push origin master
  ```

## Under the hood ...

`dcmqi` is using `publish_github_release` from [scikit-ci-addons](http://scikit-ci-addons.readthedocs.io/) to upload and manage releases from CI to GitHub.

[This section](http://scikit-ci-addons.readthedocs.io/en/latest/addons.html#testing) of the `scikit-ci-addons` documentation describes how to troubleshoot issues related to this mechanism.