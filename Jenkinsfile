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
                sh 'mvn clean package'
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
                           "target": "onlinebookstore/"
                        }
                    ]
                }'''
              )
            }
        }

        stage('deploy to tomcat container') {
            steps {
                deploy adapters: [tomcat7(credentialsId: 'tomcat', path: '', url: 'http://43.205.127.86:8088')], contextPath: null, war: '**/*.war'
            }
        }
        stage('Build Docker Image') {
            steps {
                sh '''
               docker build . --tag prasanth369/onlinebookstore:$BUILD_NUMBER
               
                '''
                
            }
        }
        stage('Push Docker Image') {
            steps {
                  withCredentials([usernamePassword(credentialsId: 'docker', passwordVariable: 'DOCKER_HUB_PASSWORD', usernameVariable: 'DOCKER_HUB_USERNAME')]) {
       
                    sh '''
                    docker login -u $DOCKER_HUB_USERNAME -p $DOCKER_HUB_PASSWORD
                        docker push prasanth369/onlinebookstore:$BUILD_NUMBER
                    '''
                  }
                }
            } 
            
     }
   
  
}

