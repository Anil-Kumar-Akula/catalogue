pipeline {
    agent {
       node {
          label 'AGENT-1'
       }
    }
    environment {
         COURSE = "jenkins"
         appVersion = ""
         ACC_ID = 193685726527
         PROJECT=  "roboshop"
         COMPONENT= "catalogue"
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
        stage ('build image') {
          steps {
            script {
              withAWS(region:'us-east-1',credentials:'aws-creds') {
                  sh """
                  aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin ${ACC_ID}.dkr.ecr.us-east-1.amazonaws.com
                  docker built -t ${ACC_ID}.dkr.ecr.us-east-1.amazonaws.com/${PROJECT}/${COMPONENT}:${appVersion}
                  docker images
                  docker push ${ACC_ID}.dkr.ecr.us-east-1.amazonaws.com/${PROJECT}/${COMPONENT}:${appVersion}
                  """
               }
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