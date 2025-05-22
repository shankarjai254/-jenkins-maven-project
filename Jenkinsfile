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
                SONAR_TOKEN = credentials('squ_6a09e137c7171ffc02064445dd3b034fa9dab4e3')
                SONAR_HOST_URL = 'http://13.201.180.176:9000/'
            }
            steps {
                withSonarQubeEnv("${SONARQUBE_ENV}") {
                    sh 'mvn -f hello-app/pom.xml sonar:sonar -Dsonar.projectKey=hello-app -Dsonar.projectName=HelloApp'
                }
            }
        }
    }
}
