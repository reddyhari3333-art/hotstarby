pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                // Checkout code from main branch
                git branch: 'main', url: 'https://github.com/reddyhari3333-art/hotstarby.git'

                // Verify files
                sh 'pwd'
                sh 'ls -l'
                sh 'ls -R'
            }
        }

        stage('Build WAR') {
            steps {
                sh 'mvn clean package'
            }
        }
        stage ('upload to nexus') {
            steps { 
                nexusArtifactUploader artifacts: [[artifactId: 'myapp', classifier: '', file: 'target/myapp.war', type: 'war']], credentialsId: '', groupId: 'in.reyaz', nexusUrl: 'http://13.201.226.65:8082/', nexusVersion: 'nexus3', protocol: 'http', repository: 'maven-releases', version: '8.3.3'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh '''
                    docker rmi -f hotstar:v1 || true
                    docker build -t hotstar:v1 -f "/var/lib/jenkins/workspace/pipeline project/Dockerfile" "/var/lib/jenkins/workspace/pipeline project"


                '''
            }
        }

        stage('Deploy Container') {
            steps {
                sh '''
                    docker rm -f con8 || true
                    docker run -d --name con8 -p 9943:8080 hotstar:v1
                '''
            }
        }
    }
}
