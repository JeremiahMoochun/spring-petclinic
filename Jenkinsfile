pipeline {
    agent any

    tools {
        maven 'Maven'    // Make sure Maven is configured in Jenkins Global Tool Configuration
        jdk 'JDK17'      // Make sure JDK is configured in Jenkins Global Tool Configuration
    }

    triggers {
        // Triggered every 5 minutes on Thursday
        cron('H/5 * * * 4')
    }

    stages {
        stage('Checkout') {
            steps {
                // Checkout source code from GitHub
                git branch: 'main', url: 'https://github.com/<your-username>/spring-petclinic.git'
            }
        }

        stage('Build') {
            steps {
                // Clean and package the application
                sh 'mvn clean package -DskipTests'
            }
        }

        stage('Unit Tests') {
            steps {
                // Run unit tests
                sh 'mvn test'
            }
            post {
                always {
                    // Publish JUnit test results
                    junit '**/target/surefire-reports/*.xml'
                }
            }
        }

        stage('Code Coverage - JaCoCo') {
            steps {
                // Generate JaCoCo code coverage report
                sh 'mvn jacoco:report'
            }
            post {
                always {
                    // Publish JaCoCo coverage report
                    jacoco(
                        execPattern: '**/target/jacoco.exec',
                        classPattern: '**/target/classes',
                        sourcePattern: '**/src/main/java',
                        exclusionPattern: '**/target/test-classes'
                    )
                }
            }
        }

        stage('Archive Artifact') {
            steps {
                // Archive the generated JAR artifact
                archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed!'
        }
        always {
            // Clean workspace after build
            cleanWs()
        }
    }
}
