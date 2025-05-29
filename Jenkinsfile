pipeline {
    agent any 
    stages {
        stage ('Build Backend') {
            steps {
                bat 'mvn clean package -DskipTests=true'
            }
        }
        stage ('Unit Tests') {
            steps {
                bat 'mvn test'
            }
        }
        stage ('Deploy Backend') {
            steps {
                deploy adapters: [tomcat8(alternativeDeploymentContext: '', credentialsId: 'TomcatLogin', path: '', url: 'http://localhost:8001/')], contextPath: 'tasks-backend', war: 'target/tasks-backend.war'
            }
        }
        stage ('API Tests') {
            steps {
                dir('api-test') {
                    git branch: 'main', url: 'https://github.com/BrunoJBorges/tasks-api-test'
                    bat 'mvn test'
                }
            }
        }
        stage ('Deploy Frontend') {
            dir('frontend') {
                steps {
                    git 'https://github.com/BrunoJBorges/tasks-frontend'
                    bat 'mvn clean package'
                    deploy adapters: [tomcat8(alternativeDeploymentContext: '', credentialsId: 'TomcatLogin', path: '', url: 'http://localhost:8001/')], contextPath: 'tasks', war: 'target/tasks.war'
                }
            }
        }
    }
}

git 'https://github.com/BrunoJBorges/tasks-frontend'