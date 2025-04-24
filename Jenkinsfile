pipeline {
    agent any
    
    environment {
        MVN_HOME = tool 'Maven'
        TOMCAT_URL = 'http://localhost:8090/manager/text' // Tomcat manager URL
    }
    
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/imhxrsh/my-webapp.git'
            }
        }
        
        stage('Build') {
            steps {
                bat "\"${MVN_HOME}\\bin\\mvn\" clean package"
            }
        }
        
        stage('Test') {
            steps {
                bat "\"${MVN_HOME}\\bin\\mvn\" test"
            }
        }
        
        stage('Deploy') {
            steps {
                withCredentials([
                    usernamePassword(
                        credentialsId: 'tomcat-creds',
                        usernameVariable: 'TOMCAT_USER',
                        passwordVariable: 'TOMCAT_PASS'
                    )
                ]) {
                    bat "\"${MVN_HOME}\\bin\\mvn\" org.codehaus.cargo:cargo-maven2-plugin:1.10.0:deploy"
                }
            }
        }
    }
    
    post {
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed!'
        }
    }
}