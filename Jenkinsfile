pipeline {
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
        stage ('Build') {
        	steps {
        		sh 'mvn package'
        	}
        }
        stage ('List files') {
        	steps {
        		sh 'ls -la'
        	}
        }
	}
}
