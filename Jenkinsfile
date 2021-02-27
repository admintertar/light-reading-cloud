pipeline {
  agent {
    node {
      label 'jnlp-slave'
    }

  }
  environment {
    build_tag = 'env-param'
  }
  stages {
    stage('Prepare') {
      steps {
        sh 'echo "1.Prepare Stage"'
        checkout scm
        script {
            tag = sh(returnStdout: true, script: 'git rev-parse --short HEAD').trim()
            build_tag = tag
            sh 'echo $tag'
            if (env.BRANCH_NAME != 'master') {
                env.build_tag = "${env.BRANCH_NAME}-${build_tag}"
            }
        }
      }
    }

    stage('Test') {
      steps {
        sh 'echo "2.Test Stage"'
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