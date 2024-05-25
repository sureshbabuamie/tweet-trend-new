pipeline {
    agent {
        node {
        label 'maven-server'
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
        stage('SonarQube analysis') {
            environment {
            scannerHome = tool 'SonarQube-scanner'
            }
                steps{
                     withSonarQubeEnv('sonar-server') { // If you have configured more than one global server connection, you can specify its name
                    sh "${scannerHome}/bin/sonar-scanner"
                    }
                }
        }   



    }   
}

