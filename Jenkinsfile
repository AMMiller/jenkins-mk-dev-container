pipeline {
    agent any

    environment { 
        // registry data
        registry = '35.226.142.211:5000' 
        registryCredential = 'nexus_deployer' 
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
                    dockerImage = docker.build registry + "/maven-agent:latest" 
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
                sh "docker rmi $registry/maven-agent:latest" 
            }
        } 
    }
}