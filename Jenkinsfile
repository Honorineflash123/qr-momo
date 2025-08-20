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
                withCredentials([string(credentialsId: 'sonar-jenkins-token1', variable: 'SONARQUBE')]) {
                    sh '''
                        sonar-scanner \
                          -Dsonar.projectKey=qr-momo \
                          -Dsonar.sources=. \
                          -Dsonar.host.url=http://sonarqube:9000 \
                          -Dsonar.login=$SONARQUBE
                    '''
                }   
            }
        }
        
        stage('Dockerizing') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'docker-hub-credentials', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                    sh 'docker login -u $USERNAME -p $PASSWORD'
    // Your Docker commands using the environment variables
                    sh 'docker build -t emmy-coming-soon1:v2 .'
                    sh 'docker tag qr-momo:v2 hnorinewehpon/qr-momo:v2'
                    sh 'docker push hnorinewehpon/qr-momo:v2'
                }
            } 
        }         
    }    
}
