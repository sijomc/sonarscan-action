You can add 2 workflows one for Develop and another one for Production. 

Add secrets from GitHub repository settings. Secret name is same as given in the *yml* file. ex: SONARQUBE_HOST

Must required secrets are SONARQUBE_HOST and SONARQUBE_TOKEN

- `SONARQUBE_HOST` - **_(Required)_** SonarQube URL.
- `SONARQUBE_TOKEN` - **_(Required)_** Authentication token of a SonarQube user. Please see [how to generate SonarQube token](https://docs.sonarqube.org/latest/user-guide/user-token/).
- `SCANNER_OPTIONS` - Please set this to "-Xmx3000m" to avoid heap memory issue. We can increase this upto 6000m. Please GitHub runner hardware resources [here](https://docs.github.com/en/free-pro-team@latest/actions/reference/specifications-for-github-hosted-runners#supported-runners-and-hardware-resources)
- `password` - using with the `login` username. Left blank if you are using authentication token.


Please see `entrypoint.sh` file for more options.


`vim .github/workflows/devl.workflow.yml`

```
on: [workflow_dispatch]
name: Development
jobs:
  sonarQubeTrigger:
    name: SonarQube Trigger
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@master
    - name: SonarQube Scan
      uses: sijomc/sonarscan-action@master
      with:
        host: ${{ secrets.SONARQUBE_HOST }}
        login: ${{ secrets.SONARQUBE_TOKEN }}
        scannerOptions: ${{ secrets.SCANNER_OPTIONS }}
        exclusions: ${{ secrets.SONAR_EXCLUSIONS }}
        projectKey: "**Develop**"
        projectName: "**Develop**"
```


`vim .github/workflows/prod.workflow.yml`


```
on: [workflow_dispatch]
name: Production
jobs:
  sonarQubeTrigger:
    name: SonarQube Trigger
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@master
    - name: SonarQube Scan
      uses: sijomc/sonarscan-action@master
      with:
        host: ${{ secrets.SONARQUBE_HOST }}
        login: ${{ secrets.SONARQUBE_TOKEN }}
        scannerOptions: ${{ secrets.SCANNER_OPTIONS }}
        exclusions: ${{ secrets.SONAR_EXCLUSIONS }}
```        