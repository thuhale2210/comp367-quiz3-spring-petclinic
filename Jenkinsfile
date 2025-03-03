pipeline {
    agent any

    triggers {
        cron('H/3 * * * 1') // Runs every 3 minutes on Mondays
    }

    tools {
        maven 'Maven3'
        jdk 'JDK11'
    }

    environment {
        JACOCO_COVERAGE_DIR = 'target/site/jacoco'
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/thuhale2210/comp367-quiz3-spring-petclinic.git'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Run Tests and Generate Code Coverage') {
            steps {
                sh 'mvn test jacoco:report'
            }
            post {
                success {
                    publishHTML([
                        allowMissing: false,
                        alwaysLinkToLastBuild: true,
                        keepAll: true,
                        reportDir: "${JACOCO_COVERAGE_DIR}",
                        reportFiles: 'index.html',
                        reportName: "Jacoco Code Coverage Report"
                    ])
                }
            }
        }

        stage('Archive Artifacts') {
            steps {
                archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
            }
        }
    }

    post {
        always {
            junit '**/target/surefire-reports/TEST-*.xml'
        }
    }
}