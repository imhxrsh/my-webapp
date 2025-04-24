pipeline {
    agent any
    
    environment {
        MVN_HOME = tool 'Maven'
        TOMCAT_URL = 'http://localhost:8090/manager/text'
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
                withCredentials([usernamePassword(credentialsId: 'tomcat-creds', usernameVariable: 'TOMCAT_USER', passwordVariable: 'TOMCAT_PASS')]) {
                    bat """
                        \"${MVN_HOME}\\bin\\mvn\" tomcat7:deploy -Dmaven.tomcat.url=${TOMCAT_URL} -Dmaven.tomcat.username=%TOMCAT_USER% -Dmaven.tomcat.password=%TOMCAT_PASS%
                    """
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