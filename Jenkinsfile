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
        sh "cd reading-cloud-gateway;docker build -t reading-cloud-gateway:${build_tag} ."
    }
    stage('Push') {
        echo "4.Push Docker Image Stage"
        withCredentials([usernamePassword(credentialsId: 'dockerHub', passwordVariable: 'dockerHubPassword', usernameVariable: 'dockerHubUser')]) {
                sh "docker login -u ${dockerHubUser} -p ${dockerHubPassword} registry-vpc.cn-shenzhen.aliyuncs.com"
                sh "docker tag reading-cloud-gateway:${build_tag} registry-vpc.cn-shenzhen.aliyuncs.com/ecs-repo/reading-cloud-gateway:${build_tag}"
                sh "docker push registry-vpc.cn-shenzhen.aliyuncs.com/ecs-repo/reading-cloud-gateway:${build_tag}"
        }
    }
    stage('YAML') {
        echo "5. Change YAML File Stage"
        sh "sed -i 's/<BUILD_TAG>/${build_tag}/' /data/light-book/reading-cloud-gateway.yaml"
    }
    stage('Deploy') {
        echo "6. Deploy Stage"
        sh "kubectl apply -f /data/light-book/reading-cloud-gateway.yaml"
    }
}