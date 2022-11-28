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
                    script{
                        withSonarQubeEnv(credentialsId: 'sonar-api-key') {
                            sh 'mvn clean package sonar:sonar'
                        }

                    }                    
                }
            }
            stage('Quality Gates Status'){

                steps{

                    script{
                        waitForQualityGate abortPipeline: false, credentialsId: 'sonar-api-key'
                        

                    }
                }
            }
            stage('Upload war file to nexus'){

                steps{

                    script{
                        nexusArtifactUploader artifacts: 
                        [
                            [artifactId: 'springboot', 
                            classifier: '', 
                            file: 'target/Uber.jar', 
                            type: 'jar'
                            ]
                        ], 
                        credentialsId: 'nexus-auth', 
                        groupId: 'com.example', 
                        nexusUrl: 'localhost:8081', 
                        nexusVersion: 'nexus3', 
                        protocol: 'http', 
                        repository: 'demoapp-release', 
                        version: '1.0.0'                    
                    }
                }
            }



        }
    
}





