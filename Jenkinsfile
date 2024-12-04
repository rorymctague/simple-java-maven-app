pipeline {
    agent {
        // Use a Docker container with Maven and JDK pre-installed
        docker {
            image 'maven:3.8.1-openjdk-11'
            args '-v /var/run/docker.sock:/var/run/docker.sock -v $WORKSPACE:/workspace'
        }
    }
    environment {
        COVERAGE_TARGET = 'target/site/jacoco' // Relative path for JaCoCo reports
    }
    stages {
        stage('Checkout') {
            steps {
                // Checkout the repository
                checkout scm
            }
        }
        stage('Build') {
            steps {
                // Run Maven clean and package
                sh 'mvn clean package -Dmaven.test.failure.ignore=true'
            }
        }
        stage('Test with JaCoCo') {
            steps {
                // Run tests and generate JaCoCo coverage reports
                sh 'mvn test'
            }
            post {
                always {
                    // Publish HTML coverage report
                    publishHTML(target: [
                        reportDir: "${env.COVERAGE_TARGET}",
                        reportFiles: 'index.html',
                        reportName: 'JaCoCo Code Coverage Report'
                    ])
                }
            }
        }
        stage('Post Build - Record Coverage') {
            steps {
                // Use the Coverage plugin to display code coverage metrics
                recordCoverage(
                    tools: [
                        jacoco(execPattern: '**/jacoco.exec')
                    ]
                )
            }
        }
    }
    post {
        always {
            // Clean up workspace after the build
            cleanWs()
        }
    }
}