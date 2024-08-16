# Jenkins
# CI/CD Pipeline

This repository contains a Jenkinsfile that defines a continuous integration and continuous deployment (CI/CD) pipeline for a Java Project application. The pipeline automates the build, test, and deployment processes, ensuring a streamlined and consistent delivery workflow.

## Pipeline Overview

The pipeline consists of the following stages:

1. **Initialize**: This stage loads the shared library and initializes the build environment.
2. **Build**: This stage builds the application using Maven and packages the artifacts.
3. **Test**: This stage executes the unit and integration tests for the application.
4. **SonarQube Analysis**: Runs the SonarQube analysis using the `mvn sonar:sonar` command and sends the analysis results to the configured SonarQube server.
5. **Dependency Scanning**: using OWASP Dependency-Check: An open-source tool for scanning Java, .NET, and Node.js dependencies. You can also use snyk.
6. **Deploy**: This stage deploys the application to the production environment.

## Prerequisites

Before running the pipeline, ensure that you have the following prerequisites set up:

- Jenkins instance with the required plugins installed (e.g., Pipeline, Git, Maven)
- Git repository containing the application source code and the Jenkinsfile
- Maven installed and configured in Jenkins
- Credentials for the production server configured in Jenkins

## SonarQube Analysis

The Jenkins pipeline includes a stage for running SonarQube analysis on the codebase. SonarQube is a code quality and security analysis tool that helps identify potential issues, technical debt, and code smells in your codebase.

In the Jenkinsfile, The `withSonarQubeEnv` function is a step provided by the SonarQube plugin for Jenkins. It is used to set up the environment variables required for running SonarQube analysis within the Jenkins pipeline. These environment variables are then used by the SonarQube Scanner for Maven ( sonar-maven-plugin) to communicate with the SonarQube server and perform the analysis

### Prerequisites

Before running the SonarQube analysis stage, ensure that you have the following prerequisites set up:

1. **SonarQube Server**: You need to have a SonarQube server instance running and accessible from your Jenkins environment. You can either set up your own SonarQube server or use a hosted SonarQube service.

2. **SonarQube Scanner for Maven**: The `sonar-maven-plugin` dependency is included in the project's `pom.xml` file, which allows you to run SonarQube analysis using Maven.

3. **SonarQube Server Configuration in Jenkins**: In your Jenkins instance, you need to configure the SonarQube server details, such as the server URL, authentication token (or username and password), and any other required information. This configuration is typically done in the "Manage Jenkins" > "Configure System" section.


### Running SonarQube Analysis

To run the SonarQube analysis stage in the Jenkins pipeline, follow these steps:

1. Set the `SONAR_HOST_URL` environment variable with the URL of your SonarQube server. You can set this variable in the Jenkins pipeline configuration or in the server environment, as described in the [Environment Variables](#environment-variables) section.

2. Run the Jenkins pipeline job. The pipeline will execute the following stages:
   - **Build**: Compiles the Java code and packages it into an executable JAR file.
   - **Test**: Executes the unit tests for the application.
   - **SonarQube Analysis**: Runs the SonarQube analysis using the `mvn sonar:sonar` command and sends the analysis results to the configured SonarQube server.

3. After the pipeline completes, you can access the SonarQube server to view the analysis results, including code quality metrics, code smells, security vulnerabilities, and technical debt.

### Customizing SonarQube Analysis

You can customize the SonarQube analysis by modifying the `sonar:sonar` command in the Jenkins pipeline or by adding properties to the `sonar-project.properties` file in your project. For example, you can exclude specific files or directories from the analysis, configure code coverage reporting, or set up quality gates.

Refer to the SonarQube documentation for more information on customizing the analysis and configuring quality gates and other advanced features.

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

#### SonarQube
[SonarQube Jenkins Info](https://www.jenkins.io/doc/pipeline/steps/sonar/)
