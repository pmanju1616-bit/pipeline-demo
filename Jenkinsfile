pipeline {
    agent any

    environment {
        MAVEN_HOME = tool name: 'maven'
        PATH = "${JAVA_HOME}\\bin;${PATH}"
    }

    stages {

        stage('Checkout Code') {
            steps {
                checkout scm
            }
        }

        stage('Build & Deploy') {
            steps {

                withCredentials([
                    usernamePassword(
                        credentialsId: 'github-packages-cred',
                        usernameVariable: 'GH_USER',
                        passwordVariable: 'GH_TOKEN'
                    )
                ]) {

                    configFileProvider([
                        configFile(
                            fileId: 'maven-github-settings',
                            variable: 'MAVEN_SETTINGS'
                        )
                    ]) {

                        bat '''
                            echo ========================================
                            echo       BUILDING AND DEPLOYING
                            echo ========================================

                            "%MAVEN_HOME%\\bin\\mvn.cmd" ^
                                -s "%MAVEN_SETTINGS%" ^
                                -B ^
                                clean deploy

                            echo.
                            echo ========================================
                            echo          DEPLOY COMPLETED
                            echo ========================================
                        '''
                    }
                }
            }
        }
    }

    post {
        success {
            echo 'Build and deployment to GitHub Packages completed successfully.'
        }

        failure {
            echo 'Pipeline failed. Check the console output for details.'
        }
    }
}
