pipeline {
    agent any
    
    tools{
        nodejs "nodejs-10"
    }

    stages {
        stage('git checkout') {
            steps {
                git branch: 'main', changelog: false, poll: false, url: 'https://github.com/abhiman619/nodejs-app-35.git'
            }
        }
        stage('install dependencies') {
            steps {
                sh 'npm install'
            }
        }
        // stage(' OWASP dependencies') {
        //     steps {
        //         dependencyCheck additionalArguments: '--scan ./ ', odcInstallation: 'DP'
        //         dependencyCheckPublisher pattern: '**/dependency-check-report.xml'
        //     }
        // }    
        stage('docker build and push ') {
            steps {
                script{
                    withDockerRegistry(credentialsId: 'c023c751-5699-475c-9961-09262bd2ffe5', toolName: 'docker') {
                        
                        sh "docker build -t demonodejs ."
                        sh "docker tag demonodejs abhiman619/nodejs:latest "
                        sh "docker push abhiman619/nodejs:latest "
                        
                        
                        }

                        
                    }
                }
            }
            stage('docker deploy ') {
            steps {
                script{
                    withDockerRegistry(credentialsId: 'c023c751-5699-475c-9961-09262bd2ffe5', toolName: 'docker') {
                        
                        sh "docker run -d --name demo-nodejs -p 8081:8081 abhiman619/nodejs:latest"

                        
                        }

                        
                    }
                }
            }

        }
    }
