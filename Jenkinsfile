pipeline {
  agent any
  tools {
    maven 'maven-3.9'
  }

  stages {
    stage('test') {
      steps {
        script {
          echo 'Testing Application'
        }
      }
    }

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
            sh 'docker build -t 164.92.67.98:8083/java-app-2.0 .'
            sh "echo ${PWD} | docker login -u ${USER} --password-stdin 164.92.67.98:8083"
            sh 'docker push 164.92.67.98:8083/java-app-2.0'
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
