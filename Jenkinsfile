pipeline{
    agent any
    environment{
        SONAR_HOME= tool "Sonar"
    }
    stages{
      
        stage("Code Checkout"){
            steps{
                git url:"https://github.com/DevOps-Playbbok/Capstone-Project-LAB.git", branch:"main"
            }
        }
        
        stage("SonarQube Code Analysis"){
            steps{
                withSonarQubeEnv("Sonar"){
                    sh "$SONAR_HOME/bin/sonar-scanner -Dsonar.projectName=wanderlust -Dsonar.projectKey=wanderlust"
                }
            }
        }
        
        stage("SonarQube Code Quality Gates"){
                steps{
                    timeout(time: 2, unit: "MINUTES"){
                        waitForQualityGate abortPipeline: false
                    }
                }
        }
        
        stage("Trivy filesystem Scan"){
            steps{
                sh "trivy fs --format table -o trivy-fs-report.html ."
            }
        }
        
        stage("Code Deploy: Docker-compose"){
            steps{    
               
                sh "docker-compose up -d"
            }
        }
    }
}