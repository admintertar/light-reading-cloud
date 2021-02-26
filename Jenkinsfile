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
            env.build_tag = sh(returnStdout: true, script: 'git rev-parse --short HEAD').trim()
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
            sh 'pwd'
            sh '''#!/bin/bash
                  cd reading-cloud-gateway
                  docker build -t reading-cloud-gateway:${env.build_tag} .'''
          }
        }

        stage('Build-reading-cloud-book') {
          steps {
            sh 'cd reading-cloud-book;docker build -t reading-cloud-book:${env.build_tag} .'
          }
        }

        stage('Build-reading-cloud-homepage') {
          steps {
            sh 'cd reading-cloud-homepage;docker build -t reading-cloud-homepage:${env.build_tag} .'
          }
        }

        stage('Build-reading-cloud-account') {
          steps {
            sh 'cd reading-cloud-account;docker build -t reading-cloud-account:${env.build_tag} .'
          }
        }

      }
    }

  }
}