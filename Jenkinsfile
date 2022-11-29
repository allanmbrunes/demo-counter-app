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

                        def readPomVersion = readMavenPom file: 'pom.xml'

                        def nexusRepo = readPomVersion.version.endsWith("SNAPSHOT") ? "demoapp-snapshot" : "demoapp-release"

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
                        repository: nexusRepo, 
                        version: "${readPomVersion.version}"                    
                    }
                }
            }

            stage ("Docker Image Build"){
                steps{
                    script{
                        sh 'docker image build -t $JOB_NAME:v1.$BUILD_ID .'
                        sh 'docker image tag $JOB_NAME:v1.$BUILD_ID allanmbstest/$JOB_NAME:v1.$BUILD_ID'
                        sh 'docker image tag $JOB_NAME:v1.$BUILD_ID allanmbstest/$JOB_NAME:v1.$BUILD_ID'
                    }
                    
                }
            }
        }
    
}





