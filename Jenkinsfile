pipeline {
    agent any

    stages {
        stage("init") {
            steps {
                script {
                    gv = load "script.groovy"
                }
            }
        }
        stage("build jar") {
            steps {
                script {
                    echo "building the simple web application"

                }
            }
        }
        stage("build image") {
            steps {
                script {
                    echo "building the docker image and restart container..."
                    //gv.buildImage()
                    withCredentials([usernamePassword(credentialsId: 'docker-private', passwordVariable: 'PASSWORD', usernameVariable: 'USERNAME')]){
                        sh 'docker build -t pm310/simple-app:ra-2.0 .'
                        sh 'docker login -u $USERNAME -p $PASSWORD'
                        sh 'docker push pm310/simple-app:ra-2.0'
                    }
                }
            }
        }
        stage("deploy") {
            steps {
                script {
                    echo "deploying the web application on ec2"
                    //gv.deployApp()

                    def dockerCmd="docker run  --name ec2-index -d pm310/simple-app:ra-2.0"
                    def dockerStop="docker stop ec2-index"
                    def dockerDelete="docker rm ec2-index"
                    sshagent(['ec2-user-key']) {
                       
                        sh "ssh -o StrictHostKeyChecking=no ec2-user@54.95.222.132 ${dockerCmd}"


                    }
                }
            }
        }
    }
}
