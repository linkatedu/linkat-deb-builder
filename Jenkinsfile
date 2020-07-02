def sourcePackage = ""

pipeline {
  environment {
    registry = "linkatedu/linkat-deb-builder"
    registryCredential = 'dockerhub'
    dockerImage = ''
  }
  agent any
  stages {
    stage('Cloning Packager Git') {
      steps {
        git 'https://github.com/linkatedu/linkat-deb-builder.git'
      }
    }
    stage('Building image') {
      steps{
        script {
          dockerImage = docker.build registry + ":$BUILD_NUMBER"
        }
      }
    }
    stage('Deploy Image') {
      steps{
        script {
          docker.withRegistry( '', registryCredential ) {
            dockerImage.push()
          }
        }
      }
    }
    
    stage('Clone another repository to subdir') {
         steps {
           sh 'rm source -rf; mkdir source'
           dir ('source') {
             git branch: 'master',
               credentialsId: 'xxxxxxxxxxxxxxxxxxxxx',
               url: 'https://github.com/deb-package-source-example.git'
            }
        }
    }
    
stage('Build') {
       steps {
         sh 'rm output -rf; mkdir output'
         sh 'docker run --rm -v /data/docker/lab/jenkins/jenkins.linkat.lab/jenkins/workspace/docker-packager-18.04/source:/package linkatedu/linkat-deb-builder:$BUILD_NUMBER -b'
       }
    }
    
   
    
  }
}
