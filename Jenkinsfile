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
pipeline {
    agent any
  environment {
      // SEMGREP_BASELINE_REF = ""

        SEMGREP_APP_TOKEN = credentials('SEMGREP_APP_TOKEN')
        SEMGREP_PR_ID = "${env.CHANGE_ID}"

      //  SEMGREP_TIMEOUT = "300"
    }
    stages {
      stage('Semgrep-Scan') {
          steps {
            sh 'pip3 install semgrep'
            sh 'semgrep ci'
          }
      }
    }
  }
