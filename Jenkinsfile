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
    }
}