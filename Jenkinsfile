pipeline {
    agent any

    stages {
        // ===== FRONTEND BUILD =====
        stage('Build Frontend') {
            steps {
                dir('reactfrontend') {
                    bat 'npm install'
                    bat 'npm run build'
                }
            }
        }

        // ===== FRONTEND DEPLOY =====
        stage('Deploy Frontend to Tomcat') {
            steps {
                bat """
                if exist "C:\\Program Files\\Apache Software Foundation\\Tomcat 10.1\\webapps\\999" (
                    rmdir /S /Q "C:\\Program Files\\Apache Software Foundation\\Tomcat 10.1\\webapps\\999"
                )

                mkdir "C:\\Program Files\\Apache Software Foundation\\Tomcat 10.1\\webapps\\999"
                xcopy /E /I /Y reactfrontend\\dist\\* "C:\\Program Files\\Apache Software Foundation\\Tomcat 10.1\\webapps\\999\\"
                """
            }
        }

        // ===== BACKEND BUILD =====
        stage('Build Backend') {
            steps {
                dir('springbootbackend') {
                    bat 'mvn clean package -DskipTests'
                }
            }
        }

        // ===== BACKEND DEPLOY =====
        stage('Deploy Backend to Tomcat') {
            steps {
                bat """
                if exist "C:\\Program Files\\Apache Software Foundation\\Tomcat 10.1\\webapps\\888.war" (
                    del /Q "C:\\Program Files\\Apache Software Foundation\\Tomcat 10.1\\webapps\\888.war"
                )
                
                if exist "C:\\Program Files\\Apache Software Foundation\\Tomcat 10.1\\webapps\\888" (
                    rmdir /S /Q "C:\\Program Files\\Apache Software Foundation\\Tomcat 10.1\\webapps\\888"
                )

                copy "springbootbackend\\target\\888.war" "C:\\Program Files\\Apache Software Foundation\\Tomcat 10.1\\webapps\\888.war"
                """
            }
        }
    }

    post {
        success {
            echo 'Deployment Successful!'
        }
        failure {
            echo 'Pipeline Failed. Check Jenkins logs.'
        }
    }
}
