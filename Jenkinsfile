pipeline {
    agent {
        node {
        label 'maven'
        }
    }
    environment {
        PATH='/usr/local/sbin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/snap/bin:/opt/apache-maven-3.9.6/bin'
    }
    stages {
	stage('build-stage') {
            steps {
	        sh 'mvn clean deploy -Dmaven.test.skip=true'
            }

        }
    }
}

