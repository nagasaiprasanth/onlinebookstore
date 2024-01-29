pipeline {
    agent any
    tools{
        maven "maven3"
    }

    stages {
        stage('git clone') {
            steps {
                git 'https://github.com/nagasaiprasanth/onlinebookstore.git'
            }
        }
         
        stage('build the code') {
            steps {
                sh 'mvn clean install'
            }
        }
        stage('Static code analysis') {
            steps {
        withSonarQubeEnv('sonarqube 8.9.10') {
                    sh  "mvn sonar:sonar"
                }
            }
                
        }
    }
}
