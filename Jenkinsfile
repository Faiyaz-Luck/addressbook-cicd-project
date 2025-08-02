pipeline {
    agent any

    environment {
        TOMCAT_HOME = "/home/ubuntu/apache-tomcat-8.5.97"
    }

    stages {
        stage('GitHub Checkout') {
            steps {
                echo "Cloning repository..."
                git url: 'https://github.com/Faiyaz-Luck/addressbook-cicd-project.git'
            }
        }

        stage('Compile') {
            steps {
                echo "Compiling the application..."
                sh 'mvn compile'
            }
        }

        stage('Test') {
            steps {
                echo "Running tests..."
                sh 'mvn test'
            }
        }

        stage('Static Code Analysis (PMD)') {
            steps {
                echo "Running static code analysis..."
                sh 'mvn pmd:pmd'
            }
        }

        stage('Package') {
            steps {
                echo "Packaging the WAR file..."
                sh 'mvn package'
            }
        }

        stage('Check WAR File') {
            steps {
                echo "Checking WAR file in target directory..."
                sh 'ls -lh target/*.war'
            }
        }

        stage('Deploy to Tomcat') {
            steps {
                echo "Deploying WAR to Tomcat..."
                // Stop Tomcat if running
                sh '''
                    if pgrep -f tomcat; then
                        echo "Stopping Tomcat..."
                        ${TOMCAT_HOME}/bin/shutdown.sh
                        sleep 5
                    fi
                '''

                // Move WAR file
                sh '''
                    echo "Moving WAR file to webapps/"
                    sudo mv -f target/addressbook.war ${TOMCAT_HOME}/webapps/
                '''

                // Set permissions
                sh '''
                    echo "Setting permissions..."
                    sudo chown -R jenkins:jenkins ${TOMCAT_HOME}
                '''

                // Start Tomcat
                sh '''
                    echo "Starting Tomcat..."
                    ${TOMCAT_HOME}/bin/startup.sh
                '''
            }
        }
    }

    post {
        failure {
            echo "Build or deployment failed."
        }
        success {
            echo "Application deployed successfully to Tomcat!"
        }
    }
}
