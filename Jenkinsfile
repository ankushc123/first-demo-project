pipeline {
    agent any
    environment {
   PATH = "/opt/apache-maven-3.8.6/bin/:$PATH" 
    }
    stages{
        stage('Git CheckOut')
        {
            steps{
                git 'https://github.com/ankushc123/first-demo-project.git' // 
                echo "Git Checkout Completed"
       
            }
        }
    }
}
