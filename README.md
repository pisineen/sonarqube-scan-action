chmod my file locally before pushing to github with :
```
chmod +x script/check-quality-gate.sh
chmod +x script/common.sh
```
run git update-index before committing my files:
```
git update-index --chmod=+x script/check-quality-gate.sh
git update-index --chmod=+x script/common.sh
```

```
on:
  # Trigger analysis when pushing in master or pull requests, and when creating
  # a pull request. 
  push:
    branches:
      - master
  pull_request:
      types: [opened, synchronize, reopened]
name: Main Workflow
jobs:
  sonarqube:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v2
      with:
        # Disabling shallow clone is recommended for improving relevancy of reporting
        fetch-depth: 0
      # Triggering SonarQube analysis as results of it are required by Quality Gate check
    - name: SonarQube Scan
      uses: sonarsource/sonarqube-scan-action@master
      env:
        SONAR_TOKEN: ${{ secrets.SONAR_TOKEN }}
        SONAR_HOST_URL: ${{ secrets.SONAR_HOST_URL }}
    - name: SonarQube Quality Gate check
      uses: pisineen/sonarqube-scan-action@main
      # Force to fail step after specific time
      timeout-minutes: 5
      env:
       SONAR_TOKEN: ${{ secrets.SONAR_TOKEN }}
   ```
Environment variables:
SONAR_TOKEN – Required this is the token used to authenticate access to SonarQube. You can read more about security tokens here. You can set the SONAR_TOKEN environment variable in the "Secrets" settings page of your repository, or you can add them at the level of your GitHub organization (recommended).
