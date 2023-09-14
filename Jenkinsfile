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
               withSonarQubeEnv('praba') {
              sh "mvn clean package sonar:sonar"
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
   stage('k8s-deploy'){
	   steps{
		   withkubeconfig(caCertificate: '', clusterName: '', contextName: '', credentialsId: 'azurepraba', namespace: '', restrictkubeconfigAccess: false, 
				  serverUrl: ''){
			   	sh 'kubectl apply -f ns.yaml'
			   	sh 'kubectl apply -f backend.yaml'
			   	sh 'kubectl apply -f frontend.yaml'
			   	sh 'kubectl apply -f backsvc.yaml'
			   	sh 'kubectl apply -f frontsvc.yaml
		   }
	   }
   }
	    post {
		    always {
			    cleanWs()
		    }
	    }
    }
 } 


