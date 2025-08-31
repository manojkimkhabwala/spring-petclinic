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
        
        stage('Deploy') {
            steps {
                sh 'sudo cp target/spring-petclinic.jar /var/lib/tomcat/webapps/'
            }
        }
    }
}
