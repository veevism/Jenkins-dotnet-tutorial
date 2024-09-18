pipeline {
    agent any

    environment {
        GITHUB_TOKEN = credentials('github-access-token') 
        NUGET_PATH = "D:/nuget.exe"  
    }

    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'main', url: 'https://github.com/veevism/Jenkins-dotnet-tutorial.git', credentialsId: 'github-access-token'
            }
        }

        stage('Restore NuGet Packages') {
            steps {
                script {
                    bat "${NUGET_PATH} restore Jenkins-dotnet-tutorial.sln"
                }
            }
        }

        stage('Build Project') {
            steps {
                script {
                    
                    bat '"C:\\Program Files (x86)MSBuild.exe" Jenkins-dotnet-tutorial.sln /p:Configuration=Release /p:Platform="Any CPU"'
                }
            }
        }
        

        stage('Copy Files') {
            steps {
                script {
                    bat '''
                    rmdir /S /Q "D:/deployment/jenkins-dotnet-tutorial" || true
                    mkdir "D:/deployment/jenkins-dotnet-tutorial"
                    xcopy /S /E /Y "Jenkins-dotnet-tutorial\\bin\\Release" "D:/deployment/jenkins-dotnet-tutorial"
                    '''
                }
            }
        }
    }

    post {
        always {
            echo 'Pipeline finished.'
        }
        success {
            echo 'Build succeeded!'
        }
        failure {
            echo 'Build failed.'
        }
    }
}
