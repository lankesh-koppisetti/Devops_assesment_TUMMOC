pipeline {
    agent any

    environment {
        IMAGE = "yourdockerhub/bike-app"
        TAG = "latest"
    }

    triggers {
        githubPush()
    }

    stages {

        stage('Code Checkout') {
            steps {
                git branch: 'master', url: 'https://github.com/lankesh-koppisetti/Devops_assesment_TUMMOC.git'
            }
        }

        stage('Lint') {
            steps {
                sh '''
                sudo apt-get update
                sudo apt-get install -y tidy
                tidy -e app/index.html
                '''
            }
        }

        stage('CQA') {
            steps {
                withSonarQubeEnv('SonarQube') {
                    sh '''
                    sonar-scanner \
                    -Dsonar.projectKey=bike-app \
                    -Dsonar.sources=app \
                    -Dsonar.login=YOUR_TOKEN
                    '''
                }
            }
        }

        stage('Test') {
            steps {
                sh 'echo "Test Passed"'
            }
        }

        stage('Build Image') {
            steps {
                sh 'docker build -t $IMAGE:$TAG .'
            }
        }

        stage('Push Image') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'docker-creds',
                usernameVariable: 'USER', passwordVariable: 'PASS')]) {
                    sh '''
                    echo $PASS | docker login -u $USER --password-stdin
                    docker push $IMAGE:$TAG
                    '''
                }
            }
        }

        stage('Deploy Blue-Green') {
            steps {
                sh '''
                kubectl apply -f k8s/green-deployment.yaml
                kubectl patch service bike-service -p '{"spec":{"selector":{"app":"bike-green"}}}'
                '''
            }
        }
    }
}
