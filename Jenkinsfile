pipeline {
    agent any
    tools {
        maven 'Maven_3.8.8_System'     // or your Maven tool name in Jenkins
        jdk 'javajenkins'            // or your configured JDK name
    }
    environment {
        SONARQUBE_ENV = 'sonarqube' // name of SonarQube server configured in Jenkins
    }
    stages {
        stage('Build') {
            steps {
                sh 'mvn -f hello-app/pom.xml -B -DskipTests clean package'
            }
            post {
                success {
                    echo "Now Archiving the Artifacts....."
                    archiveArtifacts artifacts: '**/*.jar'
                }
            }
        }

        stage('Test') {
            steps {
                sh 'mvn -f hello-app/pom.xml test'
            }
            post {
                always {
                    junit 'hello-app/target/surefire-reports/*.xml'
                }
            }
        }

        stage('SonarQube Analysis') {
            environment {
                // Add SonarQube environment variables here if needed
            }
            steps {
                withSonarQubeEnv("${SONARQUBE_ENV}") {
                    sh 'mvn -f hello-app/pom.xml sonar:sonar -Dsonar.projectKey=hello-app -Dsonar.projectName=HelloApp'
                }
            }
        }
    }
}
