podTemplate(label: 'mypod', containers: [
    containerTemplate(name: 'docker', image: 'docker', ttyEnabled: true, command: 'cat'),
    containerTemplate(name: 'kubectl', image: 'lachlanevenson/k8s-kubectl:v1.8.0', command: 'cat', ttyEnabled: true),
    containerTemplate(name: 'helm', image: 'lachlanevenson/k8s-helm:latest', command: 'cat', ttyEnabled: true),
    containerTemplate(name: 'dotnet', image: 'mcr.microsoft.com/dotnet/core/sdk:3.0', command: 'cat', ttyEnabled: true)
  ],
  volumes: [
    hostPathVolume(mountPath: '/var/run/docker.sock', hostPath: '/var/run/docker.sock'),
  ]) {
    node('mypod') {
        stage('Checkout'){
            git 'https://github.com/nikospina/app-sample-devops'
        }
        stage('Build Docker Image') {
            container('docker') {
                withCredentials([[$class: 'UsernamePasswordMultiBinding', 
                    credentialsId: 'dockerhub',
                    usernameVariable: 'DOCKER_HUB_USER', 
                    passwordVariable: 'DOCKER_HUB_PASSWORD']]) {
				    sh "docker build -t tailspin ."
				    sh "docker tag tailspin ${env.DOCKER_HUB_USER}/tailspin:${env.BUILD_NUMBER}"
                }
            }
        }
        stage('Unit Test and coverage'){
            container('dotnet') {
                sh 'dotnet test --configuration Release /p:CollectCoverage=true /p:CoverletOutputFormat=cobertura /p:CoverletOutput=./TestResults/Coverage/'
            }
        }
        stage('Publish image'){
            container('docker') {
                withCredentials([[$class: 'UsernamePasswordMultiBinding', 
                    credentialsId: 'dockerhub',
                    usernameVariable: 'DOCKER_HUB_USER', 
                    passwordVariable: 'DOCKER_HUB_PASSWORD']]) {
                    sh "docker login -u ${env.DOCKER_HUB_USER} -p ${env.DOCKER_HUB_PASSWORD}"
				    sh "docker push ${env.DOCKER_HUB_USER}/tailspin:${env.BUILD_NUMBER}"
                }
            }
        }
    }
}
