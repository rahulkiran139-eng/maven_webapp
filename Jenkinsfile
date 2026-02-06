pipeline {
    agent any

    environment {
        M2_HOME = "/usr/share/maven" // Update this if needed
        PATH = "$M2_HOME/bin:$PATH"
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/nimbuswiztech/maven_webapp.git'
            }
        }

        stage('Build with Maven') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Run Unit Tests') {
            steps {
                sh 'mvn test'
            }
        }
        
        stage('Install') {
            steps {
                sh 'mvn clean install'
            }
        }


        stage('Approval') {
            steps {
                script {
                    input message: 'Do you approve deployment to production?', ok: 'Approve'
                }
            }
        }
        
        stage('Deploy') {
            steps {
                sshagent(['tomcat']) {
                    sh 'scp -o StrictHostKeyChecking=no target/demo.war ubuntu@43.205.228.91:/home/ubuntu/'
                    sh 'ssh ubuntu@43.205.228.91 "sudo mv /home/ubuntu/demo.war /opt/tomcat/webapps/"'
                }
            }
        }
    }
}
