pipeline {
    agent
    {
        node{
        label 'mvn'
        }
    }
    environment {
   PATH = "/opt/maven/apache-maven-3.8.6/bin/:$PATH" 
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
                nexusUrl: '15.206.69.80:8081/', 
                nexusVersion: 'nexus3', 
                protocol: 'http', 
                repository: "${nexusRepoName}", 
                version: "${mavenPom.version}"
                }
        }
      }
    stage('Artifactory Pull on ansible') {
agent
    {
        node{
        label 'ans'
        }
    }
            steps {
                withCredentials([usernamePassword(credentialsId: 'nexus_id', passwordVariable: 'nexus_pass', usernameVariable: 'nuxus_user')]) {
                sh 'wget --user=$nuxus_user --password=$nuxus_pass "com/democompany/first-demo-project/4.0.0/first-demo-project-4.0.0.jar"'
                 echo 'The Artifact is Pulled Sucessfully'
                  }
             }
      }
    }
}
