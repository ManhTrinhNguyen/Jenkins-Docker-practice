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
        }
      }
    }

    stage('Increment version') {
      steps {
        script {
          echo 'Increment version ....'
          sh 'mvn build-helper:parse-version versions:set \
          -DnewVersion=\\\${parsedVersion.majorVersion}.\\\${parsedVersion.minorVersion}.\\\${parsedVersion.nextIncrementalVersion} \
          versions:commit'
          def matcher = readFile('pom.xml') =~ '<version>(.+)</version>'
          def version = matcher[0][1]
          env.IMAGE_NAME = "$version-$BUILD_NUMBER"
        }
      }
    }

    stage('build jar') {
      steps {
        script {
          echo 'Building Jar .....'
          sh 'mvn clean package'
        }
      }
    }

    stage('build docker image') {
      steps {
        script {
          withCredentials([usernamePassword(credentialsId: 'docker-hub-credentials', usernameVariable: 'USER', passwordVariable: 'PWD')]){
            sh "docker build -t nguyenmanhtrinh/demo-app:${IMAGE_NAME} ."
            sh "echo ${PWD} | docker login -u ${USER} --password-stdin"
            sh "docker push nguyenmanhtrinh/demo-app:${IMAGE_NAME}"
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

    stage('commit version update') {
      steps {
        script {
           withCredentials([usernamePassword(credentialsId: 'github-credentials', usernameVariable: 'USER', passwordVariable: 'PWD')]){
             sh 'git config --global user.email "jenkins@gmail.com"'
             sh 'git config --global user.name "jenkins"'

             sh 'git status'
             sh 'git branch'
             sh 'git config --list'

             sh "git remote set-url origin https://${USER}:${PWD}@github.com/ManhTrinhNguyen/Jenkins-Docker-practice.git"
             sh 'git add .'
             sh 'git commit -m "ci: version bump"'
             sh 'git push origin HEAD:Jenkins-Incremental-Version'
           }
        }
      }
    }
  }
}
