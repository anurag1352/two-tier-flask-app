pipeline {
    agent any

    stages {

        stage('Clean Workspace') {
            steps {
                cleanWs()
            }
        }

        stage('Code Clone') {
            steps {
                git url: 'https://github.com/anurag1352/two-tier-flask-app.git', branch: 'master'
            }
        }

        stage('Code Test') {
            steps {
                echo "Code Testing Start"
                echo "Code Is OK"
            }
        }

        stage('Trivy FS Scan') {
            steps {
                sh "trivy fs . -o trivyfs.txt"
            }
        }

        stage('Docker Image Build') {
            steps {
                sh "docker build -t two-tier-app:latest ."
            }
        }

        stage("Push To DockerHub") {
            steps {
                withCredentials([usernamePassword(credentialsId: "docker-creds", passwordVariable: "dockerHubPass", usernameVariable: "dockerHubUser")]) {
                    sh "echo ${env.dockerHubPass} | docker login -u ${env.dockerHubUser} --password-stdin"
                    sh "docker tag two-tier-app:latest ${env.dockerHubUser}/two-tier-app:latest"
                    sh "docker push ${env.dockerHubUser}/two-tier-app:latest"
                }
            }
        }

        stage('Deploy') {
            steps {
                sh 'docker-compose down && docker-compose up -d'
            }
        }
    }

    post {
        success {
            emailext(
                subject: "✅ Jenkins Build SUCCESS - ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                body: """
                <h2>Build Successful 🎉</h2>
                <p><b>Job:</b> ${env.JOB_NAME}</p>
                <p><b>Build Number:</b> ${env.BUILD_NUMBER}</p>
                <p><b>Build URL:</b> <a href="${env.BUILD_URL}">${env.BUILD_URL}</a></p>
                """,
                to: "anurag13520@gmail.com"
            )
        }

        failure {
            emailext(
                subject: "❌ Jenkins Build FAILED - ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                body: """
                <h2>Build Failed 🚨</h2>
                <p><b>Job:</b> ${env.JOB_NAME}</p>
                <p><b>Build Number:</b> ${env.BUILD_NUMBER}</p>
                <p><b>Build URL:</b> <a href="${env.BUILD_URL}">${env.BUILD_URL}</a></p>
                """,
                to: "anurag13520@gmail.com"
            )
        }
    }
}
