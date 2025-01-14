pipeline {
  agent any
  tools {
    maven 'maven-3.9'
  }

  stages {

    stage('build jar') {
      steps {
        script {
          echo 'Building Jar .....'
          sh 'mvn package'
        }
      }
    }

    stage('build docker image') {
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
      steps {
        script {
          echo "Deploying ...."
        }
      }
    }
  }
}