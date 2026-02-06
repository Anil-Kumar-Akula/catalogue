pipeline {
    agent {
       node {
          label 'AGENT-1'
       }
    }
    environment {
         COURSE = "jenkins"
         appVersion = ""
    }
    options {
        timeout(time: 10, unit: 'MINUTES') 
        disableConcurrentBuilds()
    }
   
    // This is building stage
    stages {
       stage('Read Version')  {
         steps {
           script {
             def packageJSON = readJSON file: 'package.json'
             appVersion = packageJSON.version
             echo "app version : ${appVersion}"
           }
         }
       }
       // This is Testing stage
        stage('install dependencies') { 
          steps {
            script {
              sh """
                 npm install
                 
                 """
            }
           
          }
        }
        stage {
          steps {
            script {
              sh """
                 docker build -t catalogue:${appVersion} .
                 docker images

                 """
            }
          }
        }
        stage('Deploy') {
        //    input {
        //         message "Should we continue?"
        //         ok "Yes, we should."
        //         submitter "alice,bob"
        //         parameters {
        //             string(name: 'PERSON', defaultValue: 'Mr Jenkins', description: 'Who should I say hello to?')
        //         }
        //     }

           steps {
             script {
                sh """
                echo "By using the Hybrid method deploying the pipeline"
                echo $COURSE
                """
             }
           }
        }
 
    }
    post {
        always {
            echo "I always say Hello..!"
            cleanWs()
        }
        success {
             echo "i will run if successfull"
        }
        failure {
             echo "i will run if failure"
        }
    }
     
}