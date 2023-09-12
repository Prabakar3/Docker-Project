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
	stage("sonarqube build"){
    steps{
        withCredentials([string(credentialsId: 'squ_7f086ea2b5402c1ce7e3bb824eadcd373c7e5ffc', variable: 'TOKEN')]) {
            withSonarQubeEnv('prabasonar') {
                sh "mvn clean package sonar:sonar -Dsonar.login=$TOKEN"
            }
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
	   stage('dockerbuild'){
	    steps{
  	        dir("backend"){
        	    sh 'docker build -t prabakars/backend:latest .'
                }
		dir("frontend"){
                  sh 'docker build -t prabakars/frontend:latest .'
		}
            }
        }
        stage('docker push'){
            steps{
                withCredentials([usernamePassword(credentialsId: 'dockerhub', passwordVariable: 'Praba@1234', usernameVariable: 'user')]) {
                    sh 'docker login -u prabakars -p Praba@1234'
                    sh 'docker push prabakars/backend:latest'
                    sh 'docker push prabakars/frontend:latest'
                }
            }
        } 
    } 
}

