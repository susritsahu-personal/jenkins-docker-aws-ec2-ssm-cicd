pipeline {
    agent any

    stages {
        stage('Verify Jenkins') {
            steps {
                bat 'echo Jenkins Pipeline is working!'
            }
        }

        stage('Verify Docker Access') {
            steps {
                bat '"C:\\Users\\Susrit\\AppData\\Local\\Programs\\DockerDesktop\\resources\\bin\\docker.exe" version'
            }
        }

        stage('Verify Project Files') {
            steps {
                script {
                    if (!fileExists('E:\\devops-cicd-app\\Dockerfile')) {
                        error('Dockerfile not accessible to Jenkins!')
                    }
                    echo 'SUCCESS: Jenkins can access the project Dockerfile!'
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                bat '"C:\\Users\\Susrit\\AppData\\Local\\Programs\\DockerDesktop\\resources\\bin\\docker.exe" build -t devops-cicd-app:jenkins-build1 "E:\\devops-cicd-app"'
            }
        }

        stage('Check Existing Test Container') {
            steps {
                script {
                    def result = bat(
                        returnStatus: true,
                        script: '"C:\\Users\\Susrit\\AppData\\Local\\Programs\\DockerDesktop\\resources\\bin\\docker.exe" container inspect devops-cicd-jenkins >nul 2>&1'
                    )
                    echo 'INFO: Checking whether the Jenkins test container needs replacement.'
                    echo 'INFO: Image ID comparison will be added before enabling replacement.'

                    if (result == 0) {
                        error('Safety check: devops-cicd-jenkins already exists. Deployment stopped.')
                    }
                }
            }
        }

        stage('Deploy Test Container') {
            steps {
                echo 'INFO: Deployment target is devops-cicd-jenkins on port 8083.'
                bat '"C:\\Users\\Susrit\\AppData\\Local\\Programs\\DockerDesktop\\resources\\bin\\docker.exe" run -d --name devops-cicd-jenkins -p 8083:80 devops-cicd-app:jenkins-build1'
            }
        }
    }
}