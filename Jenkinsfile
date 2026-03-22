pipeline{
    agent any
    stages{
        stage('Checkout'){
            steps{
                checkout scm
            }
        }
        stage('Build'){
            steps{
                sh 'chmod +x mvnw'
                sh './mvnw clean package -DskipTests'
            }
        }
        stage('Docker Build'){
            steps{
                sh 'docker build -t petclinic-app .'
            }
        }
        stage('Doker Run'){
            steps{
                sh 'docker rm -f petclinic || true'
                sh 'docker run -d -p 8080:8080 petclinic-app'
            }
        }
    }
}