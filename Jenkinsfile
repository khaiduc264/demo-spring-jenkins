pipeline {
  agent any

  environment {
    REGISTRY = "localhost:5000"
    IMAGE    = "demo-spring"
    TAG      = "${env.BUILD_NUMBER}"
    JAVA_HOME = "/usr/lib/jvm/java-17-openjdk-amd64"
  }

  stages {
    stage('Verify Java') {
      steps {
        sh '''
          set -e
          export PATH="$JAVA_HOME/bin:$PATH"
          echo "JAVA_HOME=$JAVA_HOME"
          java -version
          javac -version
        '''
      }
    }

    stage('Build JAR') {
      steps {
        sh '''
          set -e
          export PATH="$JAVA_HOME/bin:$PATH"
          chmod +x mvnw
          ./mvnw -q -DskipTests package
        '''
      }
    }

    stage('Build Docker Image') {
      steps {
        sh '''
          set -e
          docker build -f Dockerfile.springboot -t ${IMAGE}:${TAG} .
          docker tag ${IMAGE}:${TAG} ${REGISTRY}/${IMAGE}:${TAG}
        '''
      }
    }

    stage('Push to Local Registry') {
      steps {
        sh 'docker push ${REGISTRY}/${IMAGE}:${TAG}'
      }
    }
  }
}
