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
       

        stage("jfrog"){
            steps{
                rtUpload(
                    serverId: 'jfrog',
                    spec: '''{
                        "files": [
                            {
                                "pattern": "**/**/*.*ar",
                                "target": "project/"
                            }
                        ]
                    }'''
                )
            }
        }
	}
	}
