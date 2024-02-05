pipeline {
    agent any
    tools {
        maven "M2_HOME"
    }

    stages {
        stage('code clone') {
            steps {
                git branch: 'prasanth', url: 'https://github.com/nagasaiprasanth/onlinebookstore.git'
            }
        }
        
        stage('code build') {
            steps {
                sh 'mvn clean install'
            }
        }


        stage('Static code analysis') {
            steps {
        withSonarQubeEnv('sonarqube') {
                    sh  "mvn sonar:sonar"
                }
                }
                
            }
        
         stage('Push the artifacts into Jfrog artifactory') {
            steps {
              rtUpload (
                serverId: 'dev-server',
                spec: '''{
                      "files": [
                        {
                          "pattern": "*.war",
                           "target": "onlinestorebooks/"
                        }
                    ]
                }'''
              )
            }
        }

        stage('deploy to tomcat container') {
            steps {
                deploy adapters: [tomcat7(credentialsId: 'tomcat', path: '', url: 'http://3.110.85.37:8085')], contextPath: null, war: '**/*.war'
            }
        }
  
    }
}