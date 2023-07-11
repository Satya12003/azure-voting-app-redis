pipeline {
    agent any

    stages {
        stage('Verify Branch') {
            steps {
                echo "$GIT_BRANCH"
            }
        }
        stage('Docker Build'){
            steps{
                sh 'docker images -a'
                sh"""
                cd azure-vote/
                docker images -a
                docker build -t narasimharao12/jenkins-pipeline .
                docker images -a
                cd .. 
                """
            }
        }
        stage('Push Container'){
            steps {
                echo "WorkSpace is $WORKSPACE"
                dir("$WORKSPACE/azure-vote"){
                    script{
                           withCredentials([string(credentialsId: 'dockerhubpwd', variable: 'dockerhubpwd')]) {
                            sh 'docker login -u javatechie -p ${dockerhubpwd}'
                           }
                           sh 'docker push narasimharao12/jenkins-pipeline' 
                        }
                    }
                }
            }
        }
    }
}
