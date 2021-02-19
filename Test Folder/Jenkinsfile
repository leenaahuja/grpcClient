import groovy.transform.Field
import groovy.json.JsonOutput
def podLabel = "${UUID.randomUUID().toString()}"

pipeline {
  agent {
    kubernetes {
      		label podLabel
		yaml """
apiVersion: v1
kind: Pod
metadata:
spec:
  volumes:
  - name: dockermount
    hostPath:
     path: /var/run/docker.sock
  containers:
  - name: sapmachine
    image: gradle:4.7.0-jdk8-alpine
    command:
    - cat
    tty: true
  - name: node
    image: 'docker.wdf.sap.corp:50000/cxm/angular:v7'
    command:
    - cat
    tty: true
  - name: docker
    image: docker:18.09
    env:
     - name: NO_PROXY
       value: docker.wdf.sap.corp
    volumeMounts:
    - name: dockermount
      mountPath: /var/run/docker.sock
    command:
    - cat
    tty: true
"""
    }
  }
  stages {
    stage('Run gradle') {
      steps {
        container('gradle') {
          sh 'gradle -version'
        }
	container('node') {
          sh script: "npm run init"
	  sh 'npm run init'
	  sh script: "npm run build"
        }
      }
    }
  }
}
