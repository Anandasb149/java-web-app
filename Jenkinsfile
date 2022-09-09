pipeline {
  agent { label 'linux' }
  options {
    buildDiscarder(logRotator(numToKeepStr: '5'))
  }
  environment {
    CI=true
    ARTIFACTORY_ACCESS_TOKEN = credentials('artifactory')
  }
  stages {
    stage('Build') {
      steps {
        sh './mvn clean install '
      }
    }
    stage('Upload to Artifactory') {
      agent{
        docker{
          image'docker run releases-docker.jfrog.io/jfrog/jfrog-cli-v2-jf jf -v'
          reuseNode true 
      }
    }
    steps {
      sh 'jfrog rt upload --url http://localhost:8082/artifactory/ --access-token ${ARTIFACTORY_ACCESS_TOKEN} target/demo-0.0.1-SNAPSHOT.jar java-web-app/'
    }
    }
  }
}
