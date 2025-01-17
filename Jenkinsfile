pipeline {
  agent any
  tools {
    maven 'maven-3.9'
  }

  stages {
    stage('test') {
      steps {
        script {
          echo 'Testing Application .... '
          echo "Executing pipeline for branch $BRANCH_NAME"
        }
      }
    }

    stage('build jar') {
      // Expression log to check if that branch is main then will be execute 
      when {
        expression {
          BRANCH_NAME == 'main'
        }
      } 
      steps {
        script {
          echo 'Building Jar .....'
          sh 'mvn package'
        }
      }
    }

    stage('build docker image') {
      // Expression log to check if that branch is main then will be execute 
      when {
        expression {
          BRANCH_NAME == 'main'
        }
      } 

      steps {
        script {
          withCredentials([usernamePassword(credentialsId: 'docker-hub-credentials', usernameVariable: 'USER', passwordVariable: 'PWD')]){
            sh 'docker build -t nguyenmanhtrinh/demo-app:java-app-2.3 .'
            sh "echo ${PWD} | docker login -u ${USER} --password-stdin"
            sh 'docker push nguyenmanhtrinh/demo-app:java-app-2.3'
          }
        }
      }
    }

    stage('deploy') {
      // Expression log to check if that branch is main then will be execute 
      when {
        expression {
          BRANCH_NAME == 'main'
        }
      } 
      
      steps {
        script {
          echo "Deploying ...."
        }
      }
    }
  }
}