def project_folder = "/var/lib/jenkins/workspace/dotnetweb/hello-world-api/bin/Debug/netcoreapp2.0"
def JOB_NAME = 'DotnetSample'
def backup_folder = '/var/lib/jenkins/workspace/webbackup'
def server_folder = "${backup_folder}/${JOB_NAME}${currentBuild.number}"

pipeline {
agent any

     environment {
        def timestamp = sh(script: "echo `date +%Y-%m-%d-%H-%M-%S`", returnStdout: true).trim()
        //def timestamp = sh(script: "echo `date +%s`", returnStdout: true).trim()
    }

     options {
        timestamps()
    }
    stages {  

        stage ("Clone Repository") {
                steps {
                   git branch: 'master', url: 'https://github.com/Swadhin1997/dotnet-hello-world.git'
                }
            }  
        stage('Prep') {
            steps {
                script {
                    GIT_BRANCH=sh(returnStdout: true, script: 'git symbolic-ref --short HEAD').trim()
                    currentBuild.setDisplayName("#${currentBuild.number} [" + GIT_BRANCH + "]")
                    sh "export GIT_BRANCH=$GIT_BRANCH"
                }
            }
        }  
        stage ('Building dll') {
            steps {
               sh "dotnet build dotnet-hello-world.sln"
            }           
        }
        stage ('Copy proj to backup') {
            steps {
                script {
                    echo "Copying project folder to backup folder"
                    sh "cp -r ${project_folder} ${backup_folder}/${JOB_NAME}${currentBuild.number}_$timestamp"
                    echo "Current timestamp :: $timestamp"
                }
            }
        }
        stage ('copy proj to servers') {
            steps {
                script{
                     sh "whoami"
                     withCredentials([string(credentialsId: 'windows_passwd', variable: 'serverpasswd')]) {
                    sh "echo y | pscp -r -pw '${serverpasswd}' ${server_folder}_$timestamp Administrator@65.0.98.98:C:/Users/Administrator/Downloads/dotnetbuild"
}
                    
                }
            }
        }
        stage('Quality Analysis') {
            parallel {
                // run Sonar Scan and Integration tests in parallel.
                stage('Integration Test') {
                    steps {
                        echo 'Run integration tests here...'
                    }
                }
        stage ('Declarative Post Options') {
            steps {
                script {
                    echo 'Sending Post Build Notifications'
                }
            }  
            }
    }
}
    }
}
