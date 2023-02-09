pipeline {
   agent any
   parameters {
      choice(name: 'VERSION', choices: ['1.1.0','1.2.0','1.3.0'], description: '')
      booleanParam(name: 'executeTests', defaultValue: true, description: '')
   }
   stages {
      stage("init") {
         steps {
            script {
               gv = load "script.groovy"
            }
         }
      }
      stage("Checkout") {
         steps {
            checkout scm
         }
      }
      stage("Build") {
         steps {
            sh 'docker-compose build web'
         }
      }
      stage("test") {
         when {
            expression {
               params.executeTests
            }
         }
         steps {
            script {
               gv.testApp()
            }
         }
      }
      stage("Tag and Push") {
         steps {
                sh "docker tag jenkinspipeline_web:latest wltjd0620/jenkins-app:${BUILD_NUMBER}"
                sh "docker login -u wltjd0620 -p rlawltjd2265"
                sh "docker push wltjd0620/jenkins-app:${BUILD_NUMBER}"
         }
      }
      stage("deploy") {
         steps {
            sh "docker-compose up -d"
         }
      }
   }
}