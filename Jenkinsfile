pipeline{
    agent any
    tools{
        maven 'Maven_3.8.7'
    }
    stages{
        stage('git checkout'){
            steps{
                 checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[credentialsId: 'git credentials', url: 'https://github.com/nazreenfathi/devops.git']])
            }
        }
        stage('maven build'){
            steps{
                sh 'mvn clean install'
            }
        }
        stage('analysing in sonarqube'){
            steps{
                withSonarQubeEnv("SonarQube") {
                    sh "${tool("Sonar 4.8")}/bin/sonar-scanner \
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