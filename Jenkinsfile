pipeline{
    agent any
    stages{
        stage('Checkout'){
            steps{
                git 'https://github.com/Sarthak-code786/devops-petclinic-cicd.git'
            }
        }
        stage('Build'){
            steps{
                sh 'mvn clean package -DskipTests'
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