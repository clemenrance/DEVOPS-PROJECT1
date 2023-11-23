pipeline{

    agent any

    stages{
       
       stage("Git Download"){

        steps{
            git branch: 'main', url: 'https://github.com/clemenrance/DEVOPS-PROJECT1.git'
        }
       }
       stage("Unit Test"){

        steps{
            sh 'mvn test'
        }
       }
       stage("Integration Test"){

        steps{
            sh 'mvn verify -DskipUnitTests'
        }
       }
       stage("Maven Build"){

        steps{
            sh'mvn clean install'
        }
       }
       stage("Static Test Analysis"){

        steps{

            script{
                withSonarQubeEnv(credentialsId: 'sonarqube-token'){
                    sh 'mvn clean package sonar:sonar'
                }
            }
        }
       }
       stage("Quality Gate analysis"){

        steps{

            script{
                waitForQualityGate abortPipeline: false, credentialsId: 'sonarqube-token'
            }
        }
       }
       stage("Upload artifact to Nexus"){

        steps{

            script{
                nexusArtifactUploader artifacts: [[artifactId: 'pringboot', classifier: '', file: 'target/Uber.jar', type: 'jar']], credentialsId: 'nexus-auth', groupId: 'com.example', nexusUrl: '44.203.84.23:8081', nexusVersion: 'nexus3', protocol: 'http', repository: 'Devops-project1', version: '1.0.1'
            }
        }
       }
       stage("Docker Build"){

        steps{

            script{
             sh 'docker build -t myuber .'
            }
        }
       }
       stage("Push Image to Docker-hub"){

        steps{

            script{
                withCredentials([string(credentialsId: 'docker_auth', variable: 'docker_cred')]){
                    sh 'docker login -u clemenrance -p ${docker_cred}'
                    sh 'docker tag myuber:latest clemenrance/devops-project1:project1-image'
                    sh 'docker push clemenrance/devops-project1:project1-image'
                }
            }
        }
       }
    }
}