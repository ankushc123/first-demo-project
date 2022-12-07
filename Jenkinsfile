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
     stage('Check Code Coverage'){
               steps{
                    junit '**/target/surefire-reports/TEST-*.xml'
                    echo 'The Junit is Sucessfull'
                    jacoco()
                    echo 'The Code Coverage is Sucessfull'
                }
            }
         stage('SonarQube analysis') {
             steps {
                 script {
          // requires SonarQube Scanner 2.8+
                   scannerHome = tool 'SonarQube Scanner 4.7.0.2747'
                         }
                     withSonarQubeEnv('sonarq') {
                        sh "${scannerHome}/bin/sonar-scanner"
                        sh'mvn clean package sonar:sonar'
                          }
                        }
                     }
        }
    }

