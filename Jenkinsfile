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
        }
    
}





