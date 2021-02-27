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
        sh 'echo "Test Stage"'
        sh "BUILD_TAG = ${env.BUILD_TAG}"
      }
    }

    stage('Build-Maven') {
      steps {
        sh 'mvn clean package'
      }
    }

    stage('Build-Dockers') {
      parallel {
        stage('Build-reading-cloud-gateway') {
          steps {
            sh 'echo "${env.BUILD_TAG}"'
            sh '''#!/bin/bash
                  cd reading-cloud-gateway
                  docker build -t reading-cloud-gateway:${env.BUILD_TAG} .'''
          }
        }

        stage('Build-reading-cloud-book') {
          steps {
            sh '''#!/bin/bash
                  cd reading-cloud-book
                  docker build -t reading-cloud-book:${env.BUILD_TAG} .'''
          }
        }

        stage('Build-reading-cloud-homepage') {
          steps {
            sh '''#!/bin/bash
                  cd reading-cloud-homepage
                  docker build -t reading-cloud-homepage:${env.BUILD_TAG} .'''
          }
        }

        stage('Build-reading-cloud-account') {
          steps {
            sh '''#!/bin/bash
                  cd reading-cloud-account
                  docker build -t reading-cloud-account:${env.BUILD_TAG} .'''
          }
        }

      }
    }

  }
}