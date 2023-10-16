currentBuild.displayName = "Rishikesh-devOps-#"+currentBuild.number
pipeline {
    agent {
        label '!master'
    }
    environment {
        PATH = "/usr/local/src/apache-maven/bin:$PATH"
    }
    
    stages {
        stage ('git') {
            steps {
                git url: "https://github.com/javahometech/myweb.git"
            }
        }
        stage ('build') {
            steps {
                sh '''
                mvn clean package
                mv target/*.war target/mywebapp.war
                '''
            }
        }
                stage ('build-docker') {
                   steps {
                      sh 'docker build . -t rushikeshp/nodeapp:${DOCKER_TAG}'
                }
            }
            stage ('Dockerhub-push') {
             steps {
                 withCredentials([string(credentialsId: 'docker-hub', variable: 'dockerhubpwd')]) {
                       sh 'docker login -u rushikeshp -p ${dockerhubpwd}'
                       sh 'docker push rushikeshp/nodeapp:${DOCKER_TAG}'
          } 
           
        }
    }
}
        post {
    failure {
      // notify users when the Pipeline fails
      mail to: 'rpatil@aurusinc.com',
          subject: "Failed Pipeline: ${currentBuild.fullDisplayName}",
          body: "Something is wrong with ${env.BUILD_URL}"
    }
  }
    
}
