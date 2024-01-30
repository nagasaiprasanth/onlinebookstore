pipeline {
    agent any
    tools{
        maven "M2_HOME"
    }

    stages {
        stage('code clone') {
            steps {
                git branch: 'prasanth', credentialsId: 'github', url: 'https://github.com/nagasaiprasanth/onlinebookstore.git'
            }
        }
        stage('bulid the code ') {
            steps {
                sh 'mvn clean install'
            }
        }
        stage('static code analysis') {
            steps {
                 withSonarQubeEnv('sonarqube') {
                    sh  "mvn sonar:sonar"
                }
            }
        }
    }
}
