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
      uses: sonarsource/sonarqube-quality-gate-action@master
      # Force to fail step after specific time
      timeout-minutes: 5
      env:
       SONAR_TOKEN: ${{ secrets.SONAR_TOKEN }}
   ```

