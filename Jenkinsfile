pipeline {
    agent any
    
    triggers {
         pollSCM '* * * * *'
    }
    environment {
        CI = false          // do not treat warnings as errors
    }
    stages {
        stage('Run SonarQube Analysis') {
            steps {
                withCredentials([string(credentialsId: 'sonar-jenkins-token1', variable: 'SONAR_TOKEN')]) {
                    sh '''
                        sonar-scanner \
                          -Dsonar.projectKey=qr-momo \
                          -Dsonar.sources=. \
                          -Dsonar.host.url=http://sonarqube:9000 \
                          -Dsonar.login=$SONAR_TOKEN
                    '''
                }   
            }
        }
        
        stage('Dockerizing') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'docker-hub-credentials', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                    sh 'docker login -u $USERNAME -p $PASSWORD'
    // Your Docker commands using the environment variables
                    sh 'docker build -t qr-momo:v1 .'
                    sh 'docker tag qr-momo:v1 hnorinewehpon/qr-momo:v1'
                    sh 'docker push hnorinewehpon/qr-momo:v1'
                }
            } 
        }         
    }    
}
