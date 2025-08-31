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
                        usernameVariable: 'TOMCAT_USER',
                        passwordVariable: 'TOMCAT_PASS'
                    )]) {
                        // Define the remote commands in a single multiline string
                        def remoteCommands_pre = """
                            # Stop the Tomcat service
                            sudo systemctl stop tomcat
                            
                            # Remove the old application
                            sudo rm -rf /opt/tomcat/webapps/spring-petclinic.war
                            sudo rm -rf /opt/tomcat/webapps/spring-petclinic/
                            
                            # Start the Tomcat service
                            sudo systemctl start tomcat
                        """
                        def remoteCommands_post = """
                            # Start the Tomcat service
                            sudo systemctl start tomcat
                        """
                        // Execute the pre remote commands
                        sh "sshpass -p '${TOMCAT_PASS}' ssh ${TOMCAT_USER}@54.162.100.141 '${remoteCommands_pre}'"

                        // Use sshpass to first transfer the file and then execute the remote commands
                        sh "sshpass -p '${TOMCAT_PASS}' scp target/spring-petclinic-*.war ${TOMCAT_USER}@54.162.100.141:/opt/tomcat/webapps/"

                        // Execute the post remote commands
                        sh "sshpass -p '${TOMCAT_PASS}' ssh ${TOMCAT_USER}@54.162.100.141 '${remoteCommands_post}'"                        
                    }
                }
            }
        }
    }
}
