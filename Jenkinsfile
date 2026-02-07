pipeline {
    agent any

    environment {
        M2_HOME = "/usr/share/maven" // Update this if needed
        PATH = "$M2_HOME/bin:$PATH"
    }

    stages {
       
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
                    sh 'scp -o StrictHostKeyChecking=no target/demo.war ubuntu@13.233.184.189:/home/ubuntu/'
                    sh 'ssh ubuntu@13.233.184.189 "sudo mv /home/ubuntu/demo.war /opt/tomcat/webapps/"'
                }
            }
        }
    }
}
