pipeline {
    agent any

    environment {
        GITHUB_TOKEN = credentials('github-access-token') 
        NUGET_PATH = "D:/nuget.exe"  
        MSBUILD_PATH = "C:\\Windows\\WinSxS\\wow64_msbuild_b03f5f7f11d50a3a_4.0.15912.0_none_07ea43e35ad4fd3b\\MSBuild.exe"
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
                    
                    bat "${MSBUILD_PATH} Jenkins-dotnet-tutorial.sln /p:Configuration=Release /p:Platform=\"Any CPU\""

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
