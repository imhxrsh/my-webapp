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
                withCredentials([usernamePassword(credentialsId: 'tomcat-creds', usernameVariable: 'TOMCAT_USER', passwordVariable: 'TOMCAT_PASS')]) {
                    bat """
                        \"${MVN_HOME}\\bin\\mvn\" cargo:deploy -Dcargo.tomcat.manager.url=${TOMCAT_URL} -Dcargo.remote.username=%TOMCAT_USER% -Dcargo.remote.password=%TOMCAT_PASS%
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