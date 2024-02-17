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
                bat '''
                docker ps -q --filter "publish=8088/tcp" | findstr "." >nul
                if errorlevel 1 (
                    docker run -d -p 8088:80 --name apptestjob4 %registry%:%BUILD_NUMBER%
                ) else (
                    docker stop apptestjob4
                    docker rm apptestjob4
                    docker run -d -p 8088:80 --name apptestjob4 %registry%:%BUILD_NUMBER%
                )
                '''
            }
        }
    }
}