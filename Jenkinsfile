pipeline {
  agent {
    label 'host'
  }
  stages {
    stage('BUILD') {
      steps {
        nodejs('node') {
          echo "install dependencies"
          sh 'npm install'
          sh 'npm start & sleep 2'
          echo "access webapps before continue"
          echo "address: http://172.18.0.2:3000"
          sh 'npm run build'
        }
      }
    }
    stage('UPDATE') {
      steps {
        rtUpload (
          serverId: 'jfrogserver',
          spec: '''{
            "files": [
              {
                "pattern": "build/*",
                "target": "npm-data/nodejs-html-simple"
              }
            ]
          }'''
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
                "pattern": "npm-data/nodejs-html-simple",
                "target": "production/"
              }
            ]
            }'''
        )
      }
    }
  }
}