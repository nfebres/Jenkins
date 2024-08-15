// Load shared library
// @Library('my-shared-library') _

// Define pipeline
pipeline {
    agent any

    tools {
        maven 'Maven'
        jdk 'java'
    }

    parameters {
        string(name: 'VERSION', defaultValue: '', description: 'Version to deploy on prod')
        booleanParam(name: 'EXECUTE_TESTS', defaultValue: true, description: 'Execute tests')
    }

    environment {
        SERVER_CREDENTIALS = credentials('server-credentials')
    }

    stages {
        stage('Initialize') {
            steps {
                script {
                    gv = load 'script.groovy'
                    buildProperties = readProperties interpolate: true, file: 'build.properties'
                    buildProperties.each { prop ->
                        env."${prop.key.toUpperCase()}" = prop.value
                    }
                }
            }
        }

        stage('Build') {
            when {
                anyOf {
                    branch 'PR-*'
                    branch 'master'
                }
            }
            steps {
                script {
                    def buildVersion = getBuildVersion()
                    echo "Building the application for version ${buildVersion}"
                    buildApp(buildVersion)
                }
            }
        }

        stage('Test') {
            when {
                anyOf {
                    branch 'dev'
                    equals expected: true, actual: params.EXECUTE_TESTS
                }
            }
            steps {
                script {
                    testApp()
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    def deployVersion = params.VERSION
                    echo "Deploying version ${deployVersion}"
                    withCredentials([usernamePassword(credentialsId: env.SERVER_CREDENTIALS, usernameVariable: 'USER', passwordVariable: 'PWD')]) {
                        deployApp(deployVersion, USER, PWD)
                    }
                }
            }
        }
    }

    post {
        always {
            sendBuildNotification()
        }
        success {
            echo 'Build succeeded'
        }
        failure {
            echo 'Build failed'
        }
    }
}

// Helper functions
def getBuildVersion() {
    // Fetch the build version from a properties file or a build parameter
    return '1.0.1'
}

def buildApp(String version) {
    // Build the application using the provided version
    sh "mvn clean package -Dversion=${version}"
}

def testApp() {
    // Execute test cases
    sh 'mvn test'
}

def deployApp(String version, String user, String pwd) {
    // Deploy the application using the provided version and credentials
    sh "deploy.sh ${version} ${user} ${pwd}"
}

def sendBuildNotification() {
    // Send build notification (e.g., email, Slack message)
    mail to: 'example@example.com', subject: 'Build Notification', body: 'Build completed'
}
