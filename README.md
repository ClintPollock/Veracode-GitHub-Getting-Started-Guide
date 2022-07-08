# Veracode-GitHub-Getting-Started-Guide
How to get started with Veracode security scanning in GitHub.

For this example we will demonstrate a Static + Software Composition Analysis Policy scan.

We can use VeraDemoJava for this example.

https://github.com/veracode/verademo

## Process to Scan
* Checkout code
* Build code / create artifact
* Scan

![Create New Project](images/QuickStart-GitHub-1.png)

## Configuration Steps
* Create project
* Import code
* Create API key variables
* Paste in example yml 

### Getting Started
Create a new project, click Repos, and then click Import.  

![Create New Project](images/QuickStart-GitHub-1.png)

Click Import

![Create New Project](images/QuickStart-GitHub-2.png)

Import repository https://github.com/veracode/verademo

![Create New Project](images/QuickStart-GitHub-3.png)

Click Settings - New Repository Secret.  Add VID and VKEY with your Veracode API Credentials.

![Create New Project](images/QuickStart-GitHub-4.png)
![Create New Project](images/QuickStart-GitHub-5.png)

Click setup new workflow yourself.

![Create New Project](images/QuickStart-GitHub-6.png)


Copy in this YML


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


![Create New Project](images/QuickStart-GitHub-7.png)

Commit the Action

![Create New Project](images/QuickStart-GitHub-8.png)

Go into Actions, and then Run the Workflow

![Create New Project](images/QuickStart-GitHub-9.png)

The Workflow should complete

![Create New Project](images/QuickStart-GitHub-10.png)

You can review the logs and see that the scan has been submitted.

![Create New Project](images/QuickStart-GitHub-11.png)

Login to the Veracode platform and review results.

![Create New Project](images/QuickStart-GitHub-12.png)

## To go further, visit -

https://github.com/veracode/Veracode-manual-for-GitHub