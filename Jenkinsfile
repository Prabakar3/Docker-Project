pipeline{
    agent any
    tools{
        maven 'm1'
    }
    stages{
        stage('test'){
        
            steps{
                sh 'mvn clean test'
            }
        }
        stage('install'){
            steps{
                sh 'mvn clean install -D skipTests=true'

            }
        }
stage('sonar'){
            steps{
                withSonarQubeEnv('prabasonar') {
                    sh 'mvn clean sonar:sonar'
                }
            }
        }
	    stage("jfrog"){
            steps{
                rtUpload(
                    serverId: 'prabajfrog',
                    spec: '''{
                        "files": [
                            {
                                "pattern": "**/**/*.*ar",
                                "target": "prabajfrog/"
                            }
                        ]
                    }'''
                )
            }
        }
	}
	}
