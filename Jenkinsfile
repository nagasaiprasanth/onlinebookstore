pipeline {
    agent any
    tool {
        maven "Maven3"
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
        
         stage('Push the artifacts into Jfrog artifactory') {
            steps {
              rtUpload (
                serverId: 'dev-server',
                spec: '''{
                      "files": [
                        {
                          "pattern": "*.war",
                           "target": "web-application/"
                        }
                    ]
                }'''
              )
          }
        }
  
    }
}
