pipeline {
    agent any
    stages{
        stage('git cloned'){
            steps{
                git url:'https://github.com/karthikhr97/php-project_akshath/', branch: "master"
              
            }
        }
        stage('Build docker image'){
            steps{
                script{
                    sh 'docker build -t karthikhr97/phpprojectimg:v1.11 .'
                    sh 'docker images'
                }
            }
        }
          stage('Docker login') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'docker_cred', passwordVariable: 'PASS', usernameVariable: 'USER')]) {
                    sh "echo $PASS | docker login -u $USER --password-stdin"
                    sh 'docker push karthikhr97/phpprojectimg:v1.11'
                }
            }
        }
        
     stage('Deploy') {
            steps {
               script {
                   def dockerrm = 'sudo docker rm -f My-first-containe221 || true'
                    def dockerCmd = 'sudo docker run -itd --name My-first-containe221 -p 8082:80 karthikhr97/phpprojectimg:v1.11'
                    sshagent(['sshprivatekey']) {
                        //chnage the private ip in below code
                        // sh "docker run -itd --name My-first-containe211 -p 8082:80 karthikhr97/phpprojectimg:v1.11"
                         sh "ssh -o StrictHostKeyChecking=no ubuntu@172.31.91.190 ${dockerrm}"
                         sh "ssh -o StrictHostKeyChecking=no ubuntu@172.31.91.190 ${dockerCmd}"
                    }
                }
            }
        }
    }
}
