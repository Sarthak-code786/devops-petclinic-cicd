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
                sh './mvnw clean package -DskipTests -Dcheckstyle.skip=true'
            }
        }
        stage('SonarQube Analysis') {
            steps{
                withSonarQubeEnv('sonar-server'){
                withCredentials([string(credentialsId: 'sonar-token', variable: 'SONAR_TOKEN')]) {
                    sh '''
                    chmod +x mvnw
                    ./mvnw sonar:sonar \
                        -Dsonar.projectKey=petclinic \
                        -Dsonar.host.url=http://host.docker.internal:9000 \
                        -Dsonar.login=$SONAR_TOKEN
                    '''
                }
            }
        }
    }
        stage('Quality Gate') {
            steps {
                timeout(time: 2, unit: 'MINUTES') {
                    waitForQualityGate abortPipeline: true
                }
            }

        }
        stage('Docker Build & Push') {
    steps {
        script {
            def imageName = "sarthak786786/petclinic-app:latest"

            withCredentials([usernamePassword(credentialsId: 'docker-creds', usernameVariable: 'DOCKERHUB_USERNAME', passwordVariable: 'DOCKERHUB_PASSWORD')]) {
            sh """
            docker build -t ${imageName} .
            echo \$DOCKERHUB_PASSWORD | docker login -u \$DOCKERHUB_USERNAME --password-stdin
            docker push ${imageName}
            """
        }
    }
}
}

stage('Docker Run') {
    steps {
        sshagent(['ec2-ssh']){
        sh '''
        ssh -o StrictHostKeyChecking=no ubuntu@100.53.14.110 "
        docker rm -f petclinic || true'
        docker pull sarthak786786/petclinic-app:latest
        docker run -d -p 8080:8080 --name petclinic sarthak786786/petclinic-app:latest'
        "
        '''
    }
    }
}
        
    }
}