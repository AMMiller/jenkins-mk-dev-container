pipeline {
    agent any

    environment { 
        // registry data
        registry = '104.197.26.253:5000' 

        registryCredential = 'nexus_deployer' 
        // ${env.BUILD_NUMBER}
        imageName = "maven-agent:latest"
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
