pipeline {
  environment {
        DOCKERHUB_CREDENTIALS=credentials('haleema-dockerhub')
    }
  agent none
  stages {
stage('Git-clon & Build') {
      parallel {
    
    stage('On-Edge2-Run') {
          agent any
          steps {
            sh 'echo "edge2"'
            //git branch: 'main', url: 'https://github.com/HaleemaEssa/jenkins-edge1.git'
            sh 'docker stop -name haleema/docker-edge1' //a56987f86712; docker rm -f a56987f86712'
            //sh 'docker rmi -f haleema/docker-edge1'
            sh 'sleep 10'
            git branch: 'main', url: 'https://github.com/HaleemaEssa/jenkins-edge2.git'
            sh 'docker build -t haleema/docker-edge2:latest .'
            //sh 'docker run -v "${PWD}:/data" -t haleema/docker-edge2'
          }
    }
    stage('On-Cloud-Run') {
      agent {label 'aws'}
          steps {
            sh 'echo "cloud" '
            git branch: 'main', url: 'https://github.com/HaleemaEssa/jenkins-cloud.git'
            //sh 'docker run -v "${PWD}:/data" -t haleema/docker-cloud'
           // sh 'docker build -t haleema/docker-cloud:latest .'
          }
    }
      }
    }
stage('Login to Dockerhub') {
      parallel {

        stage('On-Edge2') {
          agent any
          steps {
            sh 'echo "edge1-run" '
            //sh 'docker stop  haleema/docker-edge1; docker rm  haleema/docker-edge1'
            sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
          }
        }

        stage('On-Cloud') {
          agent {label 'aws'}
          steps {
            sh 'echo "run-pi" '
            sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
          }
        }
        } 
      }
    stage('Running') {
      parallel {
    
    stage('On-Edge2-Run') {
          agent any
          steps {
            sh 'echo "edge2"'
            sh 'docker run -v "${PWD}:/data" -t haleema/docker-edge2'
          }
    }
    stage('On-Cloud-Run') {
      agent {label 'aws'}
          steps {
            sh 'echo "cloud" '
            sh 'docker run -v "${PWD}:/data" -t haleema/docker-cloud'
          }
    }
      }
    }
  }
}
