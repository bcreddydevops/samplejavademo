pipeline{
    agent any
    tools{
        maven 'apache-maven-3.8.6'
    }
    
      
    stages{
        stage('checkout the code'){
            steps{
                git url:'https://github.com/bcreddydevops/samplejavademo.git', branch: 'master'
            }
        }
        stage('build the code'){
            steps{
                sh 'mvn clean package'
            }
        }
         stage('Docker Build') {
            steps {
                sh "docker build . -t chinnareddaiah/samplejavademo:${commit_id()}"
            }
        }
         stage('Docker Push') {
            steps {
                withCredentials([string(credentialsId: 'docker-hub1', variable: 'hubpwd')]) {
                 sh "docker login -u chinnareddaiah -p ${hubPwd}"
                 sh "docker push chinnareddaiah/samplejavademo:${commit_id()}"
              }
            }
        }
         stage('Docker Deploy') {
            steps {
                sshagent(['docker-host']) {
                    sh "ssh -o StrictHostKeyChecking=no ec2-user@3.84.212.218 docker rm -f samplejavademo"
                    sh "ssh ec2-user@3.84.212.218 docker run -d -p 8080:8080 --name samplejavademo chinnareddaiah/samplejavademo:${commit_id()}"
                }
            }
        }
    }
}

 def commit_id(){
    id = sh returnStdout: true, script: 'git rev-parse HEAD'
    return id
    }
