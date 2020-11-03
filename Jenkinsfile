pipeline {
    environment {
        repository = "" # github url
    repositoryCredentials = "" // Jenkins cred ID
    }
    agent any
    options {
        timeout(time: 1, unit: 'HOURS)
    }
    stages {
        stage('git clone') {
            steps {
                git credentialsId: ${repositoryCredentials}
                    url: ${repository}
            }
        }
        stage('install dependency') {
            steps{
                sh 'pip install --upgrade pip'
                sh 'pip install -r requirements.txt'
            }
        }
        stage('tests') {
        steps {
            sh 'python3 -m pytest tests/'
            }
        }
        stage('create docker image') {
            steps {
                sh 'docker build -t cdn-dns-controller -f docker/Dockerfile .'
            }
        }
        stage ('run docker') {
            steps {
                sh 'docker run -it -d -p 5000:5000 --rm --name cdn-dns-controller cdn-dnc-controller'
            }
        }
        stage('sanity test') {
            steps{
                sh 'curl localhost:5000/'
            }
        }
    }
}