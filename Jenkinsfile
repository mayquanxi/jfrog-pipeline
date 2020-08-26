pipeline {
  agent {
    label 'host'
  }
  stages {
    //stage('BUILD') {
      //steps {
    //    sh "nginx -g 'daemon off;' & sleep 5"
   //   }
  //  }
    stage('PUBLISH') {
      steps {
        dockerPushStep (
          image: 'nginx:alpine',
          host: 'host',
          targetRepo: 'docker-local',
          server: 'jfrogserver',
        )
      }
    }
  }
}