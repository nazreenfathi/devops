pipeline{
    agent any
    stages{
        stage('git checkout'){
            steps{
                 checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[credentialsId: 'git credentials', url: 'https://github.com/nazreenfathi/devops.git']]) 
            }
        }
        
        stage('analysing in sonarqube'){
            steps{
                withSonarQubeEnv("Sonarqube") {
                    sh "${tool("Sonarqube 4.8.0")}/bin/sonar-scanner \
                    -Dsonar.projectKey=devops \
                    -Dsonar.java.binaries=target \
                    -Dsonar.host.url=http://http://44.203.21.133:9000 \
                    -Dsonar.login=sqp_568c971610d9b0f53681e833b698663ef4e1e306" 
                }
            }
        }
        stage('uploading artifacts in nexus'){
            steps{
                sh 'mvn -s settings.xml clean deploy'
            }
        }
    }
}