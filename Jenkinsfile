pipeline {
  agent {
    docker {
      image 'nginx:alpine'
      labels 'host'
      args '-p 8008:80'
    }
  }
  stages {
    stage('BUILD') {
      steps {
        sh "nginx -g 'daemon off;' & sleep 30"
      }
    }
    stage('PUBLISH') {
      steps {
        rtServer (
            id: 'jfrogserver',
            url: 'http://192.168.168.1:8082/artifactory/docker-local/',
            credentialsId: 'jfrogdeploy'
            )
        rtDockerPush(
            serverId: 'jfrogserver',
            image: ARTIFACTORY_DOCKER_REGISTRY + '/nginx:alpine',
            // Host:
            // On OSX: 'tcp://127.0.0.1:1234'
            // On Linux can be omitted or null
            host: HOST_NAME,
            targetRepo: 'docker-local',
            // Attach custom properties to the published artifacts:
            properties: 'project-name=docker1;status=stable',
            // If the build name and build number are not set here, the current job name and number will be used:
            //buildName: 'my-docker-build',
            //buildNumber: '17', 
            // Jenkins spawns a new java process during this step's execution.
            // You have the option of passing any java args to this new process.
            //javaArgs: '-agentlib:jdwp=transport=dt_socket,server=y,suspend=n,address=*:5005'
        )
        rtPublishBuildInfo (
            serverId: 'jfrogserver'
        )
      }
    }
  }
}