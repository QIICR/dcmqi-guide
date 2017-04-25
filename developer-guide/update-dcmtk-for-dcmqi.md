# Update DCMTK for DCMQI

Updating the DCMTK \(DICOM Toolkit\) for DCMQI is a task that takes place quite rarely. For that reason we are providing information on how to approach this task.

### HowTo
1. Fork the current dcmtk-dcmqi[^1] repository
2. Clone your forked repository:
  ``` 
  git clone 'https://github.com/{username or organization}/dcmtk-dcmqi'
  cd dcmtk-dcmqi
  git remote add dcmtk git://git.dcmtk.org/dcmtk.git 
  git fetch dcmtk
  git checkout master
  git pull dcmtk master
  git checkout dcmqi
  git rebase master
  git push origin dcmqi
  git tag DCMTK-dcmqi-{DCMTK-VERSION}_YYYYmmdd-VS12-Win64-Release-v{Release-VERSION}-static
  # example: git tag DCMTK-dcmqi-3.6.1_20170425-VS12-Win64-Release-v0.0.13-static
  git push origin {tag that you used}
  ```
3. Create a PR (pull request) from your `dcmqi` branch to the dcmtk-dcmqi[^1] repository (wait for DCMQI core developer to approve your PR.)

[^1] https://github.com/QIICR/dcmtk-dcmqi

### After PR has been approved
1. Go to AppVeyor[^2] and sign into your account via GitHub. 
2. You will be directed to the page which lists all your projects.
3. Select `dcmtk` and click `NEW BUILD` on the next page
4. After the build succeeded a new release should appear on dcmtk-dcmqi releases[^3]

[^2] https://ci.appveyor.com/login
[^3] https://github.com/QIICR/dcmtk-dcmqi/releases





