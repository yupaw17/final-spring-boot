pipeline {
    agent any

    environment {
        MAVEN_HOME = tool 'Maven'
        NEXUS_URL = "http://nexus-service:8081/repository/maven-releases/"
        NEXUS_REPO = "maven-releases"
        NEXUS_CREDENTIALS = credentials('nexus-creds')
    }

    stages {

        stage('Checkout Code') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/yupaw17/final-spring-boot.git'
            }
        }

        stage('Build JAR') {
            steps {
                sh "${MAVEN_HOME}/bin/mvn clean package -DskipTests"
            }
        }

        stage('Upload to Nexus') {
            steps {
                sh """
                ${MAVEN_HOME}/bin/mvn deploy \
                    -DskipTests \
                    -DaltDeploymentRepository=maven-releases::http://nexus-service:8081/repository/maven-releases/ \
                    -Dnexus.username=${NEXUS_CREDENTIALS_USR} \
                    -Dnexus.password=${NEXUS_CREDENTIALS_PSW}
                """
            }
        }
    }
}



