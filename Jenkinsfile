pipeline {
    agent any

    tools {
        maven 'mymaven'
    }
    stages {
        stage('code') {
            steps {
                git 'https://github.com/surendra661/rapido.git'
            }
        }
        stage('Build') {
            steps {
                echo 'building the appilication'
                sh 'mvn clean package install'

            }
        }
        stage('Test') {
            steps {
                echo 'testing the application'
                sh 'mvn test'
            }
        }
    }
}