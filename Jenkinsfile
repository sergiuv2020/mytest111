pipeline {
    agent {
        docker {
            image 'maven:3-alpine' 
            args '-v /root/.m2:/root/.m2' 
        }
    }
  stages {
    stage('CI Build and push snapshot') {
      steps {
          sh "mvn versions:set -DnewVersion=$PREVIEW_VERSION"
          sh "mvn install"
          sh "skaffold version"
          sh "export VERSION=$PREVIEW_VERSION && skaffold build -f skaffold.yaml"
          sh "jx step post build --image $DOCKER_REGISTRY/$ORG/$APP_NAME:$PREVIEW_VERSION"
          dir('charts/preview') {
            sh "make preview"
            sh "jx preview --app $APP_NAME --dir ../.."
          }
      }
      post {
          always {
              junit 'target/surefire-reports/*.xml'
          }
      }
    }
  }
  post {
        always {
          cleanWs()
        }
  }
}
