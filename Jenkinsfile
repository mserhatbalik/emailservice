    pipeline {
        agent any
        environment {
            imageName = "emailservice"
            registryCredentials = "nexus"
            dockerImage = ''
            }
        stages {
            stage('Code checkout') {
                steps {
                    checkout([$class: 'GitSCM', branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[credentialsId: 'prodevopsprojects', url: 'git@github.com:prodevopsprojects/emailservice.git']]])
                }
            }
            stage('Building image') {
                steps{
                    script {
                    dockerImage = docker.build imageName
                    }
                }
            }
            stage('Uploading to Nexus') {
                steps{  
                    script {
                        docker.withRegistry( 'http://'+registry, registryCredentials ) {
                        dockerImage.push('latest')
                        }
                    }
                }
            }
            stage('Confirming build success') {
                steps{  
                    writeFile file: 'emailservice.txt', text: 'Build is completed successfully.'
                    }
            }
        }
    }
