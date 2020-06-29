Getting started with bazel and jenkins.
https://blog.nrwl.io/dockerize-apps-with-jenkins-and-bazel-797c7964ae3b?gi=3db02254a07b

Example pipeline to run bazel: 
pipeline {
  agent any
  stages {
    stage('Publish image') {
      steps {
        sh 'bazel run //src/server:push'
      }
    }
  }
}
