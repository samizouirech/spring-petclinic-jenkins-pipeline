pipeline {
  environment {
    registry = "samiz/spring-petclinic-hub"
    registryCredential = 'docker-hub-id'
    dockerImage = ''
  }
  agent any
  tools {
    maven 'Maven 3.3.9'
    jdk 'jdk8'
  } 
  stages {
    stage('Cloning Git') {
      steps {
        git 'https://github.com/samizouirech/spring-petclinic-jenkins-pipeline.git'
      }
    }
    stage('Install Dependencies') {
      steps {
        // Maven to install dependencies
        sh 'mvn clean install -DskipTests' // Skip tests in this stage
      }
    }
    stage('Build') {
      steps {
        // Build the application
        sh 'mvn package -DskipTests' // Skip tests in this stage
      }
    }
    stage('Test') {
      steps {
        // Execute unit tests
        sh 'mvn test'
      }
    }
    stage('Building Image') {
      steps {
        script {
          // Build Docker image
          dockerImage = docker.build registry + ":latest"
        }
      }
    }
    stage('Deploy Image') {
      steps {
        script {
          // Push the Docker image to the registry
          docker.withRegistry('', registryCredential) {
            dockerImage.push()
          }
        }
      }
    }
    stage('Remove Unused docker image') {
      steps {
        // Remove the Docker image from local machine
        sh "docker rmi $registry:latest"
      }
    }
  }
}
