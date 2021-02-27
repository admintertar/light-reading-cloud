pipeline {
  agent {
    node {
      label 'jnlp-slave'
    }

  }
  environment {
    BUILD_TAG = "${sh(returnStdout: true, script: 'git rev-parse --short HEAD').trim()}"
  }

  stages {
    stage('Prepare') {
      steps {
        sh 'echo "1.Prepare Stage"'
        sh 'git pull'
      }
    }

    stage('Test') {
      steps {
        sh 'echo "2.Test Stage"'
        sh "BUILD_TAG = ${env.BUILD_TAG}"
      }
    }

    stage('Build-Maven') {
      steps {
        sh 'echo "3.Build Maven Stage"'
        sh 'mvn clean package'
      }
    }

    stage('Build-Dockers') {
      parallel {
        stage('Build-reading-cloud-gateway') {
          steps {
            sh 'echo "${build_tag}"'
            sh '''#!/bin/bash
                  cd reading-cloud-gateway
                  docker build -t reading-cloud-gateway:$build_tag .'''
          }
        }

        stage('Build-reading-cloud-book') {
          steps {
            sh '''#!/bin/bash
                  cd reading-cloud-book
                  docker build -t reading-cloud-book:$build_tag .'''
          }
        }

        stage('Build-reading-cloud-homepage') {
          steps {
            sh '''#!/bin/bash
                  cd reading-cloud-homepage
                  docker build -t reading-cloud-homepage:$build_tag .'''
          }
        }

        stage('Build-reading-cloud-account') {
          steps {
            sh '''#!/bin/bash
                  cd reading-cloud-account
                  docker build -t reading-cloud-account:$build_tag .'''
          }
        }

      }
    }

  }
}