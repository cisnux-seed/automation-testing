pipeline {
  agent any
  tools {
    maven 'Maven 3.8.8'
    jdk 'Temurin JDK 17'
  }

  environment {
    SONARQUBE_SERVER = 'SonarQube'
  }

  stages {
    stage('Checkout') {
      steps {
        git url: 'https://github.com/noghanodedra/spring-boot-rest-api-unit-tests', branch: 'main'
      }
    }

    stage('Unit Test & Coverage') {
      steps {
        sh 'mvn package'
      }
      post {
        always {
          junit 'target/surefire-reports/*.xml'
        }
      }
    }

    stage('Static Code Analysis (SAST) via Sonar') {
      steps {
        sh """
            mvn clean compile sonar:sonar \
              -Dsonar.projectKey=springboot \
              -Dsonar.projectName='springboot' \
              -Dsonar.host.url=http://sonarqube:9000 \
              -Dsonar.token=sqp_6311c9dcaed9cf76ecb99862a21d8e735b68d327
        """
      }
    }
  }

  post {
    success {
      echo "Pipeline berhasil 🚀"
    }
    failure {
      echo "Pipeline gagal 💥"
    }
  }
}
