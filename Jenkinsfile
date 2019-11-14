pipeline {
	environment {
		app = ''
		image_name = 'drake666/fin'
	}
	agent any
	tools {
		maven 'maven-3.6.2'
		jdk 'openjdk-8-jdk'
	}
	stages {
		stage ('Initialize') {
            steps {
                sh '''
                    echo "PATH = ${PATH}"
                    echo "M2_HOME = ${M2_HOME}"
                ''' 
            }
        }
        stage ('Maven Build') {
        	steps {
        		sh 'mvn package'
        	}
        }
        stage ('Build Docker Image') {
        	steps {
        		script {
        			app = docker.build(image_name)
        		}
        	}
        }
        stage ('Push Docker Image') {
        	steps {
        		script {
        			docker.withRegistry('https://registry.hub.docker.com', 'dockerhub-credentials') {
        				app.push("$BUILD_NUMBER")
        				app.push("latest")
        			}
        		}
        	}
        }
        stage ('Remove unused docker images') {
        	steps {
        		script {
        			sh '''
        				docker rmi registry.hub.docker.com/$image_name:$BUILD_NUMBER
                        docker rmi registry.hub.docker.com/$image_name:latest
                        docker image prune --force
        			'''
        		}
        	}
        }
        stage ('Deploy application to kubernetes') {
            steps {
                withKubeConfig(clusterName: 'kubernetes', contextName: 'kubernetes-admin', serverUrl: 'https://10.0.0.6:6443', credentialsId: 'kubeSecret') {
                    script {
                        sh "kubectl run --image=drake666/fin:latest fin-v$BUILD_NUMBER --port=9090 --replicas=3"
                    }
                }
            }
        }
        stage ('Expose application') {
            steps {
                withKubeConfig(clusterName: 'kubernetes', contextName: 'kubernetes-admin', serverUrl: 'https://10.0.0.6:6443', credentialsId: 'kubeSecret') {
                    script {
                        sh "kubectl expose deployment fin-v$BUILD_NUMBER --type=NodePort --port=9090 --target-port=9090"
                    }
                }
            }
        }
	}
}
