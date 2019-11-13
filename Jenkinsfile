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
		stage('SCM Checkout') {
			steps{
				git credentialsId: '6e2f84a5-6df4-416a-b57f-26d286619fba', url: 'https://github.com/forkbomb-666/jenkins-java-pipeline/'
			}
		}
	}
}
