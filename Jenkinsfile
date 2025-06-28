pipeline {
    agent any

    tools {
        maven 'mymaven'
    }

    environment {
        SONAR_TOKEN = credentials('sonar-token-id') 
    }

    stages {
        stage('Code') {
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
                echo 'Testing the application'
                sh 'mvn test'
            }
        }

        stage('CQA') {
            steps {
                withSonarQubeEnv('mysonar') {
                    sh '''
                        mvn clean verify sonar:sonar \
                        -Dsonar.projectKey=Rapido-project \
                        -Dsonar.projectName="Rapido-project" \
                        -Dsonar.login=$SONAR_TOKEN
                    '''
                }
            }
        }
    }
}
