pipeline{
    agent any
        stages{
            stage('Git Checkout'){
                
                steps{
                    git branch: 'main', url: 'https://github.com/allanmbrunes/demo-counter-app'
                }
            }
            stage('UNIT Test'){

                steps{
                    sh 'mvn test'
                }
            }
            stage('Integration Testing'){

                steps{
                    sh 'mvn verify -DskipUnitTests'
                }
            }
            stage('Maven Build'){

                steps{
                    sh 'mvn clean install'
                }
            }
            stage('Static code Analysis'){

                steps{
                    withSonarQubeEnv(credentialsId: 'sonar-api') {
                        sh 'mvn clean package sonar:sonar'
                    }
                }
            }
        }
    
}





