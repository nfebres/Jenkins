# Jenkins
# My Project CI/CD Pipeline

This repository contains a Jenkinsfile that defines a continuous integration and continuous deployment (CI/CD) pipeline for a Java Project application. The pipeline automates the build, test, and deployment processes, ensuring a streamlined and consistent delivery workflow.

## Pipeline Overview

The pipeline consists of the following stages:

1. **Initialize**: This stage loads the shared library and initializes the build environment.
2. **Build**: This stage builds the application using Maven and packages the artifacts.
3. **Test**: This stage executes the unit and integration tests for the application.
4. **Deploy**: This stage deploys the application to the production environment.

## Prerequisites

Before running the pipeline, ensure that you have the following prerequisites set up:

- Jenkins instance with the required plugins installed (e.g., Pipeline, Git, Maven)
- Git repository containing the application source code and the Jenkinsfile
- Maven installed and configured in Jenkins
- Credentials for the production server configured in Jenkins

## Running the Pipeline

To run the pipeline, follow these steps:

1. Open your Jenkins instance and create a new Pipeline job.
2. In the job configuration, select the "Pipeline script from SCM" option and provide the URL of your Git repository.
3. Configure any additional parameters required by the pipeline (e.g., version to deploy, execute tests).
4. Save the job configuration and trigger a new build.

## Customization

The Jenkinsfile can be customized to fit your specific project requirements. Here are some common customization points:

- **Build Parameters**: Modify the `parameters` section to add or remove build parameters as needed.
- **Environment Variables**: Update the `environment` section to set or retrieve environment variables required for your project.
- **Stage Conditions**: Adjust the `when` conditions for each stage based on your branching strategy or other criteria.
- **Stage Steps**: Modify the steps within each stage to include your project-specific build, test, and deployment commands.
- **Shared Library**: Update the `@Library` annotation to point to your shared library repository, if applicable.


## Setting Environment Variables

This project uses environment variables for configuring certain settings, such as the SonarQube host URL. Before running the application or the Jenkins pipeline, make sure to set the following environment variable:

- `SONAR_HOST_URL`: The URL of your SonarQube server (e.g., `http://sonarqube.example.com`).

### Local Environment

#### Unix-based systems (Linux, macOS)

```bash
export SONAR_HOST_URL=http://sonarqube.example.com
mvn clean package
```

##### For windows
```
set SONAR_HOST_URL=http://sonarqube.example.com
mvn clean package
```

##### In Jenkins Pipeline
```
pipeline {
    // ...
    stage('Build') {
        steps {
            withEnv(['SONAR_HOST_URL=http://sonarqube.example.com']) {
                sh 'mvn clean package'
            }
        }
    }
    // ...
}
```

## Jenkins Resources
### Jenkins Documentation and Tutorials

The [Jenkins website](https://www.jenkins.io/) and the [official documentation](https://www.jenkins.io/doc/) contain numerous examples and templates that cover various use cases and pipeline setups.

### GitHub Repositories

Many open-source projects on GitHub use Jenkins for continuous integration and continuous delivery (CI/CD). You can find Jenkinsfile examples in these repositories:

- [Jenkins Pipeline Examples](https://github.com/jenkinsci/pipeline-examples)
- [Jenkins Shared Library Example](https://github.com/jenkinsci/global-shared-library-example)
