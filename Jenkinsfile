pipeline{
    agent any
    stages{
        stage('Build Backend'){
            steps{
                bat 'mvn clean package -DskipTests=true'
            }
        }
        stage('Unit Tests'){
            steps{
                bat 'mvn test'
            }
        }
        stage('Deploy backend'){
            steps{
                deploy adapters: [tomcat8(credentialsId: 'TomcatLogin', path: '', url: 'http://localhost:8001/')], contextPath: 'tasks-backend', war: 'target/tasks-backend.war'
            }
        }
        stage('API Test'){
            steps{
                dir('api-test') {
                    git credentialsId: 'github_login', url: 'https://github.com/brunosilva29/tasks-api-test'
                    bat 'mvn clean test'
                }
            }
        }
        stage('Deploy Frontend'){
            steps{
                dir('frontend'){
                    git credentialsId: 'github_login', url: 'https://github.com/brunosilva29/tasks-frontend'
                    bat 'mvn clean package'
                    deploy adapters: [tomcat8(credentialsId: 'TomcatLogin', path: '', url: 'http://localhost:8001/')], contextPath: 'tasks', war: 'target/tasks.war'
                }
            }
        }
        stage('Functional-test'){
            steps{
                dir('funcional-test') {
                    git credentialsId: 'github_login', url: 'https://github.com/brunosilva29/tasks-funcional'
                    bat 'mvn clean test'
                }
            }
        }
    }
}

