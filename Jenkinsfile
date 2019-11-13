pipeline {
	agent any
	def app
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
        stage ('Build') {
        	steps {
        		sh 'mvn package'
        	}
        }
        stage ('Build Image') {
        	steps {
        		app = docker.build("forkbomb666/fin")
        	}
        }
	}
}
