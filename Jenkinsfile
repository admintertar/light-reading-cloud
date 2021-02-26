node('jnlp-slave') {
    stage('Prepare') {
        echo "1.Prepare Stage"
        checkout scm
        script {
            build_tag = sh(returnStdout: true, script: 'git rev-parse --short HEAD').trim()
                if (env.BRANCH_NAME != 'master') {
                    build_tag = "${env.BRANCH_NAME}-${build_tag}"
                }
            }
    }
    stage('Test') {
        echo "2.Test Stage"
    }
    stage('Build-Maven') {
        echo "3.Build Maven Stage"
        sh "mvn clean package"
    }
    stage('Build-Docker') {
        echo "3.Build Docker Images Stage"
        sh "docker build -f ./reading-cloud-gateway/Dockerfile -t reading-cloud-gateway:${build_tag} ."
    }
    stage('Push') {
        echo "4.Push Docker Image Stage"
    }
    stage('Deploy') {
        echo "5. Deploy Stage"
    }
}