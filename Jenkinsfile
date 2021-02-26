pipeline {
  agent {
    node {
      label 'jnlp-slave'
    }

  }
  stages {
    stage('Prepare') {
      steps {
        sh 'echo "1.Prepare Stage"'
        checkout scm
        script {
            build_tag = sh(returnStdout: true, script: 'git rev-parse --short HEAD').trim()
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
            sh 'sh "cd reading-cloud-gateway;docker build -t reading-cloud-gateway:${build_tag} ."'
          }
        }

        stage('Build-reading-cloud-book') {
          steps {
            sh 'sh "cd reading-cloud-book;docker build -t reading-cloud-book:${build_tag} ."'
          }
        }

      }
    }

  }
}