def registry = 'https://sureshbabuamie01.jfrog.io/'
def imagename = 'sureshbabuamie01.jfrog.io/docker-trial/myapp'
def version = '2.1.2'
pipeline {
    agent {
        node {
            label 'maven-server'
        }
    }
environment {
    PATH = "/opt/apache-maven-3.9.4/bin:$PATH"
}
    stages {
        stage("build"){
            steps {
                 echo "----------- build started ----------"
                sh 'mvn clean deploy -Dmaven.test.skip=true'
                 echo "----------- build complted ----------"
            }
        }
        stage("test"){
            steps{
                echo "----------- unit test started ----------"
                sh 'mvn surefire-report:report'
                 echo "----------- unit test Complted ----------"
            }
        }
  
         
        stage("Jar Publish") {
        steps {
            script {
                    echo '<--------------- Jar Publish Started --------------->'
                     def server = Artifactory.newServer url:registry+"/artifactory" ,  credentialsId:"jforg-cred"
                     def properties = "buildid=${env.BUILD_ID},commitid=${GIT_COMMIT}";
                     def uploadSpec = """{
                          "files": [
                            {
                              "pattern": "jarstaging/(*)",
                              "target": "suresh-libs-release-local/{1}",
                              "flat": "false",
                              "props" : "${properties}",
                              "exclusions": [ "*.sha1", "*.md5"]
                            }
                         ]
                     }"""
                     def buildInfo = server.upload(uploadSpec)
                     buildInfo.env.collect()
                     server.publishBuildInfo(buildInfo)
                     echo '<--------------- Jar Publish Ended --------------->'  
            
                }
            }   
        }
        stage ("docker build") {
          steps {
            script {
                 echo "-------------------building the docker file starts-------------------------"
                app = docker.build(imagename +":" + version)
            }                                
               }
            }

        stage ("docker push") {
          steps {
            script {
                 echo "-------------------pushing the docker file starts-------------------------"
                 docker.withRegistry(registry,'jforg-cred') {
                    app.push()
                 }
                }                                
               }
            }    
        }

    }



    

