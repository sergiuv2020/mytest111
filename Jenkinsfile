pipeline {
    agent {
        kubernetes {
              label "maven"
              // -dind"
              // inheritFrom "maven"
              // yamlFile "maven-dind.yaml"
        }
    }
    parameters {
      string(name: 'URL', defaultValue: 'http://google.com', description: 'Url to do a GET request')
    }
    stages {
      stage('RUN ci Test') {
        steps {
          container('maven') {
            sh "mvn clean test -Durl=${params.URL}"
          }
        }
      }
    }
}
