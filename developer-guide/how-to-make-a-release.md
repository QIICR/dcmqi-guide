# maintainers: how to make a release ?

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
