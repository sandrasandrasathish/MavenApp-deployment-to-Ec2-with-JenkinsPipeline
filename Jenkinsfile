pipeline {
    agent any

    tools {
        maven 'maven-3.9'
        jdk 'JDK11'
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/sandrasandrasathish/MavenApp-deployment-to-Ec2-with-JenkinsPipeline.git'
            }
        }

        stage('Build') {
            steps {
                bat 'mvn clean package'
            }
        }

        stage('Deploy to EC2') {
            steps {
                sshagent(['ec2-ssh-key']) {
                    bat '''
                    echo Copying JAR to EC2...
                    scp -o StrictHostKeyChecking=no target/demo-1.0.0.jar ubuntu@54.174.123.249:/opt/app/

                    echo Starting application on EC2...
                    ssh -o StrictHostKeyChecking=no ubuntu@54.174.123.249 "pkill -f demo-1.0.0.jar || true"

                    ssh -o StrictHostKeyChecking=no ubuntu@54.174.123.249 "nohup java -jar /opt/app/demo-1.0.0.jar > /opt/app/app.log 2>&1 &"
                    '''
                }
            }
        }
    }

    post {
        success {
            echo "Deployment completed successfully."
        }
        failure {
            echo "Deployment failed. Check Jenkins or EC2 logs."
        }
    }
}
