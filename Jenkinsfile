pipeline {
  agent {
    node {
      label 'jnlp-slave'
    }

  }
  environment {
    build_tag = 'env-param'
  }
  parameters {
      string(name: 'PERSON', defaultValue: 'Jenkins', description: 'Who should I say hello to?')
  }
  stages {
    stage('Prepare') {
      steps {
        sh 'echo "1.Prepare Stage"'
        checkout scm
        script {
            params.PERSON = sh(returnStdout: true, script: 'git rev-parse --short HEAD').trim()
            if (env.BRANCH_NAME != 'master') {
                build_tag = "${env.BRANCH_NAME}-${build_tag}"
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
            sh 'cd reading-cloud-gateway;docker build -t reading-cloud-gateway:${params.PERSON} .'
          }
        }

        stage('Build-reading-cloud-book') {
          steps {
            sh 'cd reading-cloud-book;docker build -t reading-cloud-book:${params.PERSON} .'
          }
        }

        stage('Build-reading-cloud-homepage') {
          steps {
            sh 'cd reading-cloud-homepage;docker build -t reading-cloud-homepage:${params.PERSON} .'
          }
        }

        stage('Build-reading-cloud-account') {
          steps {
            sh 'cd reading-cloud-account;docker build -t reading-cloud-account:${params.PERSON} .'
          }
        }

      }
    }

  }
}