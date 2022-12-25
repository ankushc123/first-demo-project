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
                script{
                  def mavenPom = readMavenPom file: 'pom.xml'
                  def nexusRepoName = mavenPom.version.endsWith("SNAPSHOT") ? "Demo_Snapshot" : "Demo_Release"
            nexusArtifactUploader artifacts: 
                [
                    [artifactId: "${mavenPom.artifactId}", 
                     classifier: '', 
                     file: "target/first-demo-project-${mavenPom.version}.jar", 
                     type: 'jar']
                ], 
                credentialsId: 'nexus_cred', 
                groupId: "${mavenPom.groupId}", 
                nexusUrl: '3.108.193.241:8081/', 
                nexusVersion: 'nexus3', 
                protocol: 'http', 
                repository: "${nexusRepoName}", 
                version: "${mavenPom.version}"
                }
        }
      }
     }
   }

