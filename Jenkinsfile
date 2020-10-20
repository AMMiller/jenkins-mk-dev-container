pipeline {
    agent any

    environment { 
        // registry data
        registry = '35.239.186.113:5000' 
        registryCredential = 'nexus_deployer' 
        imageName = "maven-agent:${env.BUILD_NUMBER}"
        dockerImage = '' 

    }
    stages {
        stage ('Fetch Dockerfile') {
            steps {
                git 'https://github.com/AMMiller/docker-maven'
            }
        }
        stage('Building docker image') { 
            steps { 
                script { 
                    dockerImage = docker.build registry + '/$imageName'
                }
            } 
        }
        stage('Deploy docker image') { 
            steps { 
                script { 
                    docker.withRegistry( 'http://' + registry, registryCredential ) { 
                        dockerImage.push() 
                    }
                } 
            }
        } 
        stage('Cleaning up') { 
            steps { 
                sh 'docker rmi $registry/$imageName'
            }
        } 
    }
}