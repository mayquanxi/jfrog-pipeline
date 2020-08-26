pipeline {
  agent {
    label 'host'
  }
  stages {
    stage('BUILD') {
      steps {
        rtNpmResolver (
            id: 'resolver-id',
            serverId: 'jfrogserver',
            repo: 'npm-local-dependencies'
        )
        rtNpmDeployer (
            id: 'deployer-id',
            serverId: 'jfrogserver',
            repo: 'npm-local'
            // Attach custom properties to the published artifacts:
            //properties: ['key1=value1', 'key2=value2']
        )
        rtNpmInstall (
            // Optional tool name from Jenkins configuration
            tool: 'node',
            // Optional path to the project root. If not set, the root of the workspace is assumed as the root project path.
            path: '.',
            // Optional npm flags or arguments.
            //args: '--verbose',
            resolverId: 'resolver-id',
            buildName: 'nodejs-example'
            // Jenkins spawns a new java process during this step's execution.
            // You have the option of passing any java args to this new process.
            //javaArgs: '-agentlib:jdwp=transport=dt_socket,server=y,suspend=n,address=*:5005'
        )
      }
    }
    stage('PUBLISH') {
      steps {
        rtNpmPublish (
            // Optional tool name from Jenkins configuration
            tool: 'node',
            // Optional path to the project root. If not set, the root of the workspace is assumed as the root project path.
            path: '.',
            deployerId: 'deployer-id',
            // Jenkins spawns a new java process during this step's execution.
            // You have the option of passing any java args to this new process.
            //javaArgs: '-agentlib:jdwp=transport=dt_socket,server=y,suspend=n,address=*:5005'
        )
      }
    }
    stage('DOWNLOAD') {
      steps {
        rtDownload (
          serverId: 'jfrogserver',
          spec: '''{
            "files": [
              {
                "pattern": "npm-data/",
                "target": "production/"
              }
            ]
            }'''
        )
        sh 'cd production'
        sh 'npm install'
        sh 'npm start & sleep 30'
      }
    }
  }
}