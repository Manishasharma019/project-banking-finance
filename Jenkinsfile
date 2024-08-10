pipeline{
    agent any
    stages{
        stage('checkout the code from github'){
            steps{
                 git url: 'https://github.com/Manishasharma019/project-banking-finance'
                 echo 'github url checkout'
            }
        }
        stage('codecompile'){
            steps{
                echo 'starting compiling'
                sh 'mvn compile'
            }
        }
        stage('codetesting '){
            steps{
                sh 'mvn test'
            }
        }
        stage('qa '){
            steps{
                sh 'mvn checkstyle:checkstyle'
            }
        }
        stage('package'){
            steps{
                sh 'mvn package'
            }
        }
        stage('run dockerfile'){
          steps{
               sh 'docker build -t manisha0109/financemeproject:1.1 .'
           }
         }
        stage('Login to dockerhub'){
            steps{
                withCredentials([string(credentialsId: 'dockerhubcredential', variable: 'dockerhubcredential')]) {
                    sh 'docker login -u manisha0109 -p ${dockerhubcredential}'
                
                }
            }
        }
        stage('Push the image to dockerhub'){
            steps{
                sh 'docker push manisha0109/financemeproject:1.1'
            }
        }
        stage('Deployment stage using ansible'){
            steps{
                ansiblePlaybook become: true, becomeUser: null, credentialsId: 'ansible', disableHostKeyChecking: true, installation: 'ansible', inventory: '/etc/ansible/hosts', playbook: 'ansible-playbook.yml', sudoUser: null, vaultTmpPath: ''
            }
        }
    }
}
