pipeline {
    agent any
    tools {
        maven 'Maven_3.8.8_System'     // or your Maven tool name in Jenkins
        jdk 'javajenkins'            // or your configured JDK name
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
        SONAR_TOKEN = credentials('adminsonar')
        SONAR_HOST_URL = 'http://13.203.66.197:9000/'
    }
    steps {
        sh '''
            mvn -f hello-app/pom.xml sonar:sonar \
                -Dsonar.projectKey=hello-app \
                -Dsonar.projectName=HelloApp \
                -Dsonar.host.url=$SONAR_HOST_URL \
                -Dsonar.login=$SONAR_TOKEN
        '''
    }
}

    }
}
