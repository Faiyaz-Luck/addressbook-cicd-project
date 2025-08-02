pipeline {
    agent any

    stages {
        stage('GitHub Checkout') {
            steps {
                git url: 'https://github.com/Faiyaz-Luck/addressbook-cicd-project.git'
            }
        }

        stage('Compile') {
            steps {
                sh 'mvn compile'
            }
        }

        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }

        stage('Static Code Analysis (PMD)') {
            steps {
                sh 'mvn pmd:pmd'
            }
        }

        stage('Package') {
            steps {
                sh 'mvn package'
            }
        }

        stage('Check WAR File') {
            steps {
                sh 'ls -lh target/'
            }
        }

        stage('Deploy to Tomcat') {
            steps {
                // Ensure Jenkins user has write access to Tomcat's webapps/ folder
                sh 'mv target/addressbook.war /home/ubuntu/apache-tomcat-8.5.100/webapps/'
            }
        }
    }
}
