pipeline {
    agent any

    tools {
        maven 'Maven'
        jdk 'JDK17'
    }

    triggers {
        
        cron('H/5 * * * 4')
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/JeremiahMoochun/spring-petclinic.git'
            }
        }

        stage('Build & Test with JaCoCo') {
            steps {
                
                sh './mvnw clean package -Dspring-javaformat.skip=true -Dcheckstyle.skip=true'
            }
            post {
                always {
                    junit '**/target/surefire-reports/*.xml'

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
    }
}
