pipeline {
    agent any

    tools {
        maven "maven-3.8.4"
    }
    stages {
        stage('Git Checkout') {
            steps {
                
                git branch: 'main', url: 'https://github.com/Aatmesh/IA.git'

            }
        }
        stage('clean and install') {
            steps {
               sh 'mvn clean install'
            }
        }
        stage('package') {
            steps {
               sh 'mvn package'
            }
        }
        stage('Server') {
            steps {
               rtServer (
                  id: "JFrog_Artifactory",
                  url: 'http://192.168.3.164:8082/artifactory',
                  username: 'admin',
                  password: 'Devops@123',
                  bypassProxy: true,
                  timeout: 300
               )
            }
        }
       stage('Upload') {
            steps {
                rtUpload (
                    serverId: 'JFrog_Artifactory',
                        spec: '''{
                            "files": [
                                {
                                "pattern": "*.war",
                                "target": "aatmesh-maven-test-libs-snapshot"
                                }
                            ]
                        }''',
                )
            }
        }
        stage('publish build info') {
            steps {
               rtPublishBuildInfo (
                   serverId: "JFrog_Artifactory" 
               )
            }
        }
    }
}
