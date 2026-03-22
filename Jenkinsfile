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
        stage('Docker Build & Push') {
    steps {
        script {
            def DOCKERHUB_USERNAME = 'sarthak786786'
            def DOCKERHUB_PASSWORD = 'Sarthak@786786'
            def imageName = "${DOCKERHUB_USERNAME}/petclinic-app:latest"

            sh "docker build -t ${imageName} ."
            sh "docker login -u ${DOCKERHUB_USERNAME} -p ${DOCKERHUB_PASSWORD}"
            sh "docker push ${imageName}"
        }
    }
}

stage('Docker Run') {
    steps {
        sh 'docker rm -f petclinic || true'
        sh 'docker run -d -p 8080:8080 --name petclinic sarthak786786/petclinic-app:latest'
    }
}
        
    }
}