pipeline {
  agent {
    node {
      label 'jnlp-slave'
    }

  }
  environment {
    BUILD_TAG = "${sh(returnStdout: true, script: 'git rev-parse --short HEAD')}"
  }

  stages {

    stage('Build-Maven') {
      steps {
        sh 'echo "${env.BUILD_TAG}"'

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