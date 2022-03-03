pipeline {
    agent none
    tools{
        jdk 'myjava'
        maven 'mymaven'
    }
    stages {
        stage('COMPILE') {
            agent any
            steps {
                script{
                    echo "COMPILING THE CODE"
                    git 'https://github.com/siri-rishi/addressbook.git'
                    sh 'mvn compile'
               }
                          }     
            }
         stage('UNITTEST') {
             agent{label 'linux_slave'}
            steps {
                script{
                    echo "RUNNING THE UNIT TEST CASES"
                    sh 'mvn test'
                    }
                    
            } 
            post{
                always{
                    junit 'target/surefire-reports/*.xml'
                }
            }    
            }
            stage('PACKAGE'){
                agent any
                steps{
                    script{
                        sshagent(['Test_server-key']) {

                        echo "PACKAGING THE CODE"
                        sh "scp -o strictHostkeyChecking=no server-script.sh ec2-user@172.31.41.142:/home/ec2-user"
                        sh "ssh -o strictHostkeyChecking=no ec2-user@172.31.41.142 'bash ~/server-script.sh'"

                        }
                    }
                }
           }
             }
}
