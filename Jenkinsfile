pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                echo 'Starting build...'
                git 'https://github.com/harshita9104/pipeline.git'
                sh "docker build -t harsh9104/pipeline:${env.BUILD_NUMBER}.0 ."
                echo 'Build completed'
            }
        }
        stage('Push') {
            steps {
                echo 'Starting push...'
                sh "docker login -u harsh9104 -p password"
                sh "docker push harsh9104/pipeline:${env.BUILD_NUMBER}.0"
                echo 'Push completed' 
            }
        }
        stage('Version') {
            steps {
                echo 'Starting update...'
                git 'https://github.com/harshita9104/pipeline-yaml.git'
                sh "sed -i 's|image: harsh9104/pipeline:.*|image: harsh9104/pipeline:${env.BUILD_NUMBER}.0|' deployment.yaml"
                sh "cat deployment.yaml"
                sh "git config user.name 'harshita roonwal'"
                sh "git config user.email 'roonwal721972@gmail.com'"
                sh "git add ."
                sh "git commit -m 'Jenkins build version ${env.BUILD_NUMBER}.0'"
                sh "git config --local credential.helper '!f() { echo username=HarshitaRoonwal; echo password=PAT; }; f'"
                sh "git push origin master"
                echo 'Update completed'   
            }
        }
    }
}
