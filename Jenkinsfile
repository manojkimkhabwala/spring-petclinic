pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/manojkimkhabwala/spring-petclinic.git'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Deploy to Tomcat') {
            steps {
                script {
                    withCredentials([usernamePassword(
                        credentialsId: 'ssh',
                        usernameVariable: 'TOMCAT_USER',    // A variable to hold the username
                        passwordVariable: 'TOMCAT_PASS'     // A variable to hold the password
                    )]) {
                        // The code inside this block has access to the variables
                        // The password will be masked in the build logs
                        sh "sshpass -p '${TOMCAT_PASS}' scp target/spring-petclinic.war ${TOMCAT_USER}@54.162.100.141:/opt/tomcat/webapps/"
                    }
                }
            }
        }
    }
}
