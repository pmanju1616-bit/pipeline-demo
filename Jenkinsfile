pipeline {

    agent any

    tools {
        maven 'maven'
        jdk 'JDK'
    }

    environment {
        GITHUB_CREDS = credentials('github-package-creds')
    }

    stages {

        stage('Checkout Code') {
            steps {
                checkout scm
            }
        }

        stage('Build & Deploy') {
            steps {
                configFileProvider(
                    [configFile(
                        fileId: 'MyGlobalSettings',
                        variable: 'MAVEN_SETTINGS'
                    )]
                ) {
                    bat """
                        echo ========================================
                        echo       BUILDING MAVEN PROJECT
                        echo ========================================

                        mvn -B clean deploy -s "%MAVEN_SETTINGS%"
                    """
                }
            }
        }
    }

    post {
        success {
            echo 'Build and deployment to GitHub Packages completed successfully.'
        }

        failure {
            echo 'Build or deployment failed.'
        }
    }
}
