pipeline {
    agent any
    stages {
        /*stage('Build') { 
            steps {
                sh 'mvn -B -DskipTests clean package' 
            }
        }
        stage('Test') {
            steps {
                sh 'mvn test'
            }
            post {
                always {
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }*/

        stage('Build and Test') {
            steps {
                script {
                    // Run Maven build, which includes tests and JaCoCo coverage generation
                    sh "mvn clean verify"
                }
            }
        }

        stage('Publish Coverage Report') {
            steps {
                script {
                    // Use the Coverage Plugin to publish JaCoCo reports (JaCoCo .exec or .xml file)
                    publishCoverage(
                        adapters: [jacocoAdapter(coverageReportFile: '**/target/jacoco.xml')],  // Use JaCoCo XML report
                        sourceFileResolver: sourceFiles('**/src/main/java')
                    )
                }
            }
        }

        stage('Publish HTML Report') {
            steps {
                // Use HTML Publisher plugin to show JaCoCo HTML report in Jenkins
                publishHTML(
                    target: [
                        reportName: 'JaCoCo Coverage Report',
                        reportDir: 'target/site/jacoco',
                        reportFiles: 'index.html'
                    ]
                )
            }
        }
    }

    post {
        always {
            // Clean up or other post-actions
        }
    }
}