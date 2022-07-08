# Veracode-GitHub-Getting-Started-Guide
How to get started with Veracode security scanning in GitHub.

For this example we will demonstrate a Static + Software Composition Analysis Policy scan.

We can use VeraDemoJava for this example.

https://github.com/veracode/verademo

## Process to Scan
* Checkout code
* Build code / create artifact
* Scan

![Create New Project](images/QuickStart-GitHub-1)

## Configuration Steps
* Create project
* Import code
* Create API key variables
* Paste in example yml 

### Getting Started
Create a new project, click Repos, and then click Import.  

![Create New Project](images/GitLab-Getting-Started-1.png)

Import code from:

https://github.com/veracode/verademo

![Import Code](images/GitLab-Getting-Started-2.png)

Click Settings, CI/CD, and then find the Variables section to add your Veracode vid and vkey.

![Add Veracode API Keys](images/GitLab-Getting-Started-3.png)

Create Pipeline and import below yml
![Create Pipeline and Import below yml](images/GitLab-Getting-Started-4.png)


```bash
name: Verademo Scanning Example Github

on:
  workflow_dispatch:
  repository_dispatch:
    types: [test]

jobs:
  checkout-package-scan:

    runs-on: ubuntu-latest

    steps:
    - uses: actions/checkout@v2
    - name: Set up JDK 1.8
      uses: actions/setup-java@v1
      with:
        java-version: 1.8
  
    - name: Build with Maven
      run: mvn -f app/pom.xml clean package 

    - name: Get the Java API wrapper
      uses: wei/curl@master
      with:
        args: -sS -o VeracodeJavaAPI.jar "https://repo1.maven.org/maven2/com/veracode/vosp/api/wrappers/vosp-api-wrappers-java/19.6.5.8/vosp-api-wrappers-java-19.6.5.8.jar"

    - name: Start SAST scan
      run: java -jar VeracodeJavaAPI.jar -action uploadandscan -vid ${{ secrets.VERACODE_API_ID }} -vkey ${{ secrets.VERACODE_API_KEY }} -appname Github-VeraDemo -createprofile false -version "GitHub Actions job $GITHUB_RUN_NUMBER" -filepath /home/runner/work/VeraDemoJava/VeraDemoJava/app/target/verademo.war
```

Once you save the GitHub Pipeline it will checkout the code, build and artifact the app, and then submit the application for a Static + Software Composition Analysis scan.  

![Create Pipeline and Import below yml](images/GitLab-Getting-Started-5.png)

Check the platform to review the results.

## To go further, visit -

https://gitlab.com/veracode-gitlab-manual/veracode-manual-for-gitlab