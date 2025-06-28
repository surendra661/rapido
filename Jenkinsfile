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
                sh 'mvn clean package'
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

        stage('Deploy to Tomcat') {
            steps {
                echo 'Deploying WAR file to Tomcat Server...'
                deploy adapters: [tomcat9(
                    alternativeDeploymentContext: '', 
                    credentialsId: 'tomcat', 
                    path: '', 
                    url: 'http://54.88.246.116:8080'
                )], 
                contextPath: null, 
                war: 'target/*.war'
                
                echo 'Deployment completed successfully!'
            }
        }
    }
}