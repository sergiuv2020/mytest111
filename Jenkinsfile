pipeline {
    agent {
        kubernetes {
              // Change the name of jenkins-maven label to be able to use yaml configuration snippet
              label "maven-dind"
              // Inherit from Jx Maven pod template
              inheritFrom "maven"
              // Add pod configuration to Jenkins builder pod template
              yamlFile "maven-dind.yaml"
        }
    }
    stages {
      stage('CI Build and push snapshot') {
        // when {
        //   branch 'PR-*'
        // }
        environment {
          PREVIEW_VERSION = "0.0.0-SNAPSHOT-$BRANCH_NAME-$BUILD_NUMBER"
        }
        steps {
          container('maven') {
            sh "mvn versions:set -DnewVersion=$PREVIEW_VERSION"
            sh "mvn clean install test"
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
