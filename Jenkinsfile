pipeline {
    agent any
    stages{
        stage('Build'){
            steps {
                sh 'mvn clean package'
            }
            post {
                success {
                    echo 'Now Archiving...'
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }
        stage ('Deploy to Staging'){
            steps {
                build job: 'D2S'
            }
        }
        stage ('Deploy to Production'){
            steps {
                timeout(time:5, unit:'DAYS'){
                    input message: 'Approve PRODUCTION Deployemnt?'
                }
                build job: 'D2P'
            }
            post {
                success {
                    echo 'Code Deployed to Production'
                }
                failure {
                    echo 'Deployment failed'
                }
            }
        }

    }
}