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
        stage('Upload Artifact to Nexus'){
        steps{
            nexusArtifactUploader artifacts: 
                [
                    [artifactId: 'first-demo-project', 
                     classifier: '', 
                     file: 'target/first-demo-project-0.0.1-SNAPSHOT.jar', 
                     type: 'jar']
                ], 
                credentialsId: 'nexus_cred', 
                groupId: 'com.democompany', 
                nexusUrl: '3.108.193.241:8081', 
                nexusVersion: 'nexus3', 
                protocol: 'http', 
                repository: 'Demo_Snapshot', 
                version: '0.0.1-SNAPSHOT'
        }
      }
     }
   }

