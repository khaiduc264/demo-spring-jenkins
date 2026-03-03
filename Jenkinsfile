pipeline {
  agent any

  environment {
    JAVA_HOME = "/usr/lib/jvm/java-21-openjdk-amd64"
    PATH = "${JAVA_HOME}/bin:${env.PATH}"
    REGISTRY = "localhost:5000"
    IMAGE    = "demo-spring"
    TAG      = "${env.BUILD_NUMBER}"
  }

  stages {
    stage('Checkout') {
      steps {
        checkout scm
      }
    }

    stage('Build JAR') {
      steps {
        sh 'chmod +x mvnw'
        sh './mvnw -q -DskipTests package'
      }
    }

    stage('Build Docker Image') {
      steps {
        sh 'docker build -f Dockerfile.springboot -t ${IMAGE}:${TAG} .'
        sh 'docker tag ${IMAGE}:${TAG} ${REGISTRY}/${IMAGE}:${TAG}'
      }
    }

    stage('Push to Local Registry') {
      steps {
        sh 'docker push ${REGISTRY}/${IMAGE}:${TAG}'
      }
    }
  }
}
