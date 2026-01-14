pipeline{
    
    agent { label "dev" }
    stages{
        stage("Code clone"){
            steps{
                echo "Code Cloning......"
                git url: "https://github.com/anurag1352/two-tier-flask-app.git", branch: "master"
                echo "Cloning Done......"
            }
        }
        stage("Build Image"){
            steps{
                echo "Image Build start.."
                sh "docker build -t two-tier-app ."
                echo "Image Build Sucessfully.."
                
            }
        }
        stage("Code Test"){
            steps{
                echo "Testing Start"
                echo "Testing done..."
            }
        }
        stage("Scan Image & Files"){
            steps{
                sh "trivy fs . -o results.json"
            }
        }
        stage("Deployment"){
            steps{
                echo "Deployment Start.."
                sh "docker-compose down && docker-compose up -d"
                echo "Deployment Done....."
            }
        }
    }
}
