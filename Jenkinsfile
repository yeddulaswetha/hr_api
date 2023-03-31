pipeline {
    agent any

    stages {
        stage('git checkout') {
            steps {
                git branch: 'main', credentialsId: 'apitoken', url: 'https://github.com/yeddulaswetha/hr_api'
            }
        }

        stage('maven') {
            steps {
                sh 'mvn clean package'
            }
        }
        stage('tomcat dev') {
            steps {
                sshagent(['sshkey']) {
                    sh "scp -o StrictHostKeyChecking=no target/hr-api.war ec2-user@44.193.211.179 /opt/tomcat9/webapps/"
                    sh "ssh ec2-user@44.193.211.179 /opt/tomcat9/bin/shutdown.sh"
                    sh "ssh ec2-user@44.193.211.179 /opt/tomcat9/bin/startup.sh"
                }
            }
        }
    }
    post{
        always{
    cleanWs()
        }
    }
}
