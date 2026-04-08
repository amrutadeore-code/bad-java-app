pipeline {
    agent { label 'windows' }

    tools {
        jdk 'JDK8'
        maven 'Maven3'
    }
	
	environment {
      // SEMGREP_BASELINE_REF = ""

        SEMGREP_APP_TOKEN = credentials('SEMGREP_APP_TOKEN')
        SEMGREP_PR_ID = "${env.CHANGE_ID}"
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
		
		stage('Install Semgrep') {
            steps {
                bat '"C:\\Users\\deore\\AppData\\Local\\Microsoft\\WindowsApps\\python.exe" -m pip install --upgrade pip'
                bat '"C:\\Users\\deore\\AppData\\Local\\Microsoft\\WindowsApps\\python.exe" -m pip install semgrep'
            }
        }
		
		stage('Semgrep Scan') {
            steps {
                bat 'semgrep ci'
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