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
                echo 'Compiling the project...'
                sh 'mvn compile'
            }
        }

        stage('Test') {
            steps {
                echo 'Running unit tests...'
                sh 'mvn test'
            }
        }

        stage('Static Code Analysis (PMD)') {
            steps {
                echo 'Skipping PMD temporarily to avoid hang...'
                // Uncomment below after fixing PMD config in pom.xml
                // sh 'mvn pmd:pmd'
            }
        }

        stage('Package') {
            steps {
                echo 'Packaging the project into WAR...'
                sh 'mvn package'
            }
        }

        stage('Check WAR File') {
            steps {
                echo 'Checking generated WAR file...'
                sh 'ls -lh target/'
            }
        }

        stage('Deploy to Tomcat') {
            steps {
                echo 'Deploying WAR to Tomcat...'
                // Make sure this path exists and has write access
                sh 'mv target/addressbook.war /home/ubuntu/apache-tomcat-8.5.97/webapps/'
            }
        }
    }
}
