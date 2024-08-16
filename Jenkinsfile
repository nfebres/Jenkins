pipeline {
    agent any

    tools {
        maven 'Maven'
        jdk 'java'
    }

    environment {
        SONAR_QUBE_SERVER = 'Your SonarQube Server'
        DEPENDENCY_CHECK_TOOL = 'OWASP Dependency-Check'
        SERVER_CREDENTIALS = 'your-server-credentials-id'
    }

    stages {
        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv(env.SONAR_QUBE_SERVER) {
                    sh 'mvn sonar:sonar'
                }
            }
        }

        stage('Dependency Scanning') {
            steps {
                dependencyCheck additionalArguments: '--format HTML --format XML', odcInstallation: env.DEPENDENCY_CHECK_TOOL
            }
            post {
                always {
                    publishHTML([
                        allowMissingReports: false,
                        alwaysLinkToLastBuild: false,
                        keepAll: true,
                        reportDir: 'target',
                        reportFiles: 'dependency-check-report.html',
                        reportName: 'OWASP Dependency Report',
                        reportTitles: ''
                    ])
                    archiveArtifacts artifacts: 'target/dependency-check-report.xml'
                }
            }
        }

        // stage('Dependency Scanning') {
        //     steps {
        //         script {
        //             // Running the dependency scanning tool
        //             sh 'snyk test --json > snyk_report.json'
        //         }
        //     }
        //     post {
        //         always {
        //             archiveArtifacts artifacts: 'snyk_report.json', fingerprint: true
        //         }
        //         failure {
        //             error 'Dependency scanning failed with vulnerabilities'
        //         }
        //     }
        // }

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
    return '1.0.1'
}

def buildApp(String version) {
    sh "mvn clean package -Dversion=${version}"
}

def testApp() {
    sh 'mvn test'
}

def deployApp(String version, String user, String pwd) {
    sh "deploy.sh ${version} ${user} ${pwd}"
}

def sendBuildNotification() {
    // Send build notification
    mail to: 'example@example.com', subject: 'Build Notification', body: 'Build completed'
}
