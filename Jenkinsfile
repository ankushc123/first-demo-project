pipeline {
    agent {
        node{
        label 'java-build-node'
        }
}
    environment {
   PATH = "/opt/apache-maven-3.8.6/bin/:$PATH" 
    }
    stages{
        stage('Git CheckOut')
        {
            steps{
                script{
                 git 'https://github.com/ankushc123/first-demo-project.git'
                }
            }
        }
         stage('Unit Test'){
                steps{
                    sh 'mvn test'
                }
            }
      stage('Build The Code'){
                steps{
                    script{
                   sh 'mvn clean install -DskipTest'
                          }
                     }
       }
    }
}
