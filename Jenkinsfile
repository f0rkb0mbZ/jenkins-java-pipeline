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
        			docker.withRegistry('https://docker.pkg.github.com', '6e2f84a5-6df4-416a-b57f-26d286619fba') {
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
        				docker images
        			'''
        		}
        	}
        }
	}
}
