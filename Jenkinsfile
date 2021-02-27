node('jnlp-slave') {
    stage('Prepare') {
        checkout scm
        script {
            build_tag = sh(returnStdout: true, script: 'git rev-parse --short HEAD').trim()
                if (env.BRANCH_NAME != 'master') {
                    build_tag = "${env.BRANCH_NAME}-${build_tag}"
                }
            }
    }
    stage('Build-Maven') {
        sh "mvn clean package"
    }
    stage('Build-Dockers'){

        sh "cd reading-cloud-gateway;docker build -t reading-cloud-gateway:${build_tag} ."
        sh "cd reading-cloud-book;docker build -t reading-cloud-book:${build_tag} ."
        sh "cd reading-cloud-account;docker build -t reading-cloud-account:${build_tag} ."
        sh "cd reading-cloud-homepage;docker build -t reading-cloud-homepage:${build_tag} ."

    }
    stage('Push') {
        withCredentials([usernamePassword(credentialsId: 'dockerHub', passwordVariable: 'dockerHubPassword', usernameVariable: 'dockerHubUser')]) {
                sh "docker login -u ${dockerHubUser} -p ${dockerHubPassword} registry-vpc.cn-shenzhen.aliyuncs.com"

                sh "docker tag reading-cloud-gateway:${build_tag} registry-vpc.cn-shenzhen.aliyuncs.com/ecs-repo/reading-cloud-gateway:${build_tag}"
                sh "docker push registry-vpc.cn-shenzhen.aliyuncs.com/ecs-repo/reading-cloud-gateway:${build_tag}"

                sh "docker tag reading-cloud-book:${build_tag} registry-vpc.cn-shenzhen.aliyuncs.com/ecs-repo/reading-cloud-book:${build_tag}"
                sh "docker push registry-vpc.cn-shenzhen.aliyuncs.com/ecs-repo/reading-cloud-book:${build_tag}"

                sh "docker tag reading-cloud-account:${build_tag} registry-vpc.cn-shenzhen.aliyuncs.com/ecs-repo/reading-cloud-account:${build_tag}"
                sh "docker push registry-vpc.cn-shenzhen.aliyuncs.com/ecs-repo/reading-cloud-account:${build_tag}"

                sh "docker tag reading-cloud-homepage:${build_tag} registry-vpc.cn-shenzhen.aliyuncs.com/ecs-repo/reading-cloud-homepage:${build_tag}"
                sh "docker push registry-vpc.cn-shenzhen.aliyuncs.com/ecs-repo/reading-cloud-homepage:${build_tag}"
        }
    }

    stage('YAML') {
        sh "sed -i 's/<BUILD_TAG>/${build_tag}/' /data/light-book/reading-cloud-gateway.yaml"
        sh "sed -i 's/<BUILD_TAG>/${build_tag}/' /data/light-book/reading-cloud-book.yaml"
        sh "sed -i 's/<BUILD_TAG>/${build_tag}/' /data/light-book/reading-cloud-account.yaml"
        sh "sed -i 's/<BUILD_TAG>/${build_tag}/' /data/light-book/reading-cloud-homepage.yaml"
    }
    stage('Deploy') {
        sh "kubectl apply -f /data/light-book/reading-cloud-gateway.yaml"
        sh "kubectl apply -f /data/light-book/reading-cloud-book.yaml"
        sh "kubectl apply -f /data/light-book/reading-cloud-account.yaml"
        sh "kubectl apply -f /data/light-book/reading-cloud-homepage.yaml"


        sh "sed -i 's/${build_tag}/<BUILD_TAG>/' /data/light-book/reading-cloud-gateway.yaml"
        sh "sed -i 's/${build_tag}/<BUILD_TAG>/' /data/light-book/reading-cloud-book.yaml"
        sh "sed -i 's/${build_tag}/<BUILD_TAG>/' /data/light-book/reading-cloud-account.yaml"
        sh "sed -i 's/${build_tag}/<BUILD_TAG>/' /data/light-book/reading-cloud-homepage.yaml"

    }
}