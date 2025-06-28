pipeline {
    agent any

    tools {
        maven 'mymaven' // Make sure this Maven tool is configured in Jenkins global tools
    }

    environment {
        // If you're using a token, you can add it like this, or set it inside Jenkins credentials and use with credentials block.
        // SONAR_TOKEN = credentials('sonar-token-id')
    }

    stages {
        stage('Code Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/surendra661/rapido.git'
            }
        }

        stage('Build') {
            steps {
                echo 'Building the application'
                sh 'mvn clean package install'
            }
        }

        stage('Test') {
            steps {
                echo 'Running unit tests'
                sh 'mvn test'
            }
        }

        stage('CQA') {
            steps {
                echo 'Running SonarQube Code Quality Analysis'
                withSonarQubeEnv('mysonar') { // 'mysonar' should match your SonarQube server name in Jenkins config
                    sh '''
                        mvn sonar:sonar \
                        -Dsonar.projectKey=rapido \
                        -Dsonar.projectName="Rapido Project"
                    '''
                }
            }
        }
    }
}
