pipeline {
    agent { label 'windows' }

    tools {
        jdk 'JDK8'
        maven 'Maven3'
    }

    options {
        timestamps()
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Verify Java and Maven') {
            steps {
                bat 'java -version'
                bat 'mvn -version'
            }
        }

        stage('Build WAR') {
            steps {
                withMaven(jdk: 'JDK8', maven: 'Maven3') {
                    bat 'mvn -B clean package -DskipTests'
                }
            }
        }
    }

    post {
        always {
            archiveArtifacts artifacts: 'target\\**\\*.war', fingerprint: true, allowEmptyArchive: true
        }
    }
}