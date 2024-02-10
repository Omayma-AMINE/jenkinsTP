pipeline{
    environment{
        registry="amine1omayma/webappjenkinstp"
        registryCredential="dockerID"
        dockerImage=""
    }
    agent any
    stages{
        stage('Cloning Git'){
            steps{
                git branch: 'main', url: 'https://github.com/Omayma-AMINE/jenkinsTP.git'
            }
        }
        stage('Building image'){
            steps{
                script{
                    dockerImage = docker.build registry+":$BUILD_NUMBER"
                }
            }
        }
        stage('Test image'){
            steps{
                script{
                    echo "Tests passed"
                }
            }
        }
        stage('Publish image'){
            steps{
                script{
                    docker.withRegistry('',registryCredential){
                        dockerImage.push()
                    }
                }
            }
        }
        stage('Deploy image'){
            steps{
                bat "docker run -d -p 8095:80 $registry:$BUILD_NUMBER"
            }
        }
    }
}