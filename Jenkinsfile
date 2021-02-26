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
      }
    }

    stage('Test') {
      steps {
        sh 'echo "2.Test Stage"'
      }
    }

    stage('Build-Maven') {
      steps {
        sh '''echo "3.Build Maven Stage"
sh "mvn clean package"'''
      }
    }

    stage('Build-Dockers') {
      parallel {
        stage('Build-Dockers') {
          steps {
            sh 'echo "3.Build Docker Images Stage"'
            sh 'sh "cd reading-cloud-gateway;docker build -t reading-cloud-gateway:${build_tag} ."'
            sh 'sh "cd reading-cloud-book;docker build -t reading-cloud-book:${build_tag} ."'
          }
        }

        stage('') {
          steps {
            sh 'sh "cd reading-cloud-book;docker build -t reading-cloud-book:${build_tag} ."'
          }
        }

      }
    }

  }
}