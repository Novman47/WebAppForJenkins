pipeline {
    agent any
	
	stages {
	    stage('Git CheckOut') {
		    steps {
			   checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/Novman47/WebAppForJenkins.git']])
			}
		}
        stage('Clean and Install') {
            steps {
                sh 'mvn clean install'
            }
        }
        stage ('Package'){
            steps {
                sh 'mvn package'
             }
        }
	stage ('Server'){
            steps {
               rtServer (
                 id: "my_jfrog-sever",
                 url: 'http://172.31.18.40:8081/artifactory',
                 credentialsId: 'default-deployer',
                 bypassProxy: true,
                 timeout: 300
               )
            }
        }
        stage('Upload'){
            steps{
                rtUpload (
                 serverId:"my_jfrog-sever" ,
                  spec: '''{
                   "files": [
                      {
                      "pattern": "*.war",
                      "target": "my_repo/"
                      }
                            ]
                           }''',
                        )
            }
        }
        stage ('Publish build info') {
            steps {
                rtPublishBuildInfo (
                    serverId: "my_jfrog-sever"
                )
            }
        }
    }
}
