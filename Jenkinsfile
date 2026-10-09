pipeline {
    agent {
        node {
            label 'prod'
        }
    }

    stages {

        stage('CODE') {
            steps {
                git 'https://github.com/Chakri-79/python-code-library-app.git'
            }
        }

        stage('CQA') {
            environment {
                scannerHome = tool 'mysonar'
            }
            steps {
                withSonarQubeEnv('mysonar') {
                    sh "${scannerHome}/bin/sonar-scanner -Dsonar.projectKey=python-sonar"
                }
            }
        }

        stage('BUILD') {
            steps {
                sh '''
                    docker build -t chakradhar098/py-lib-app:dbimage database/
                    docker build -t chakradhar098/py-lib-app:authimage auth/
                    docker build -t chakradhar098/py-lib-app:bookimage book/
                    docker build -t chakradhar098/py-lib-app:borrowimage borrow/
                    docker build -t chakradhar098/py-lib-app:appimage .
                '''
            }
        }

        stage('SCAN') {
            steps {
                sh '''
                    trivy image chakradhar098/py-lib-app:dbimage
                    trivy image chakradhar098/py-lib-app:authimage
                    trivy image chakradhar098/py-lib-app:bookimage
                    trivy image chakradhar098/py-lib-app:borrowimage
                    trivy image chakradhar098/py-lib-app:appimage
                '''
            }
        }

        stage('REGISTRY') {
            steps {
                withDockerRegistry(credentialsId: 'dockerhub-cred') {
                    sh '''
                        docker push chakradhar098/py-lib-app:dbimage
                        docker push chakradhar098/py-lib-app:authimage
                        docker push chakradhar098/py-lib-app:bookimage
                        docker push chakradhar098/py-lib-app:borrowimage
                        docker push chakradhar098/py-lib-app:appimage
                    '''
                }
            }
        }

        stage('DEPLOY') {
            steps {
                sh 'docker stack deploy -c compose.yml library'
            }
        }
    }
}