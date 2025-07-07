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
          // Archive test results and reports
          archiveArtifacts artifacts: 'target/surefire-reports/**/*', allowEmptyArchive: true
          archiveArtifacts artifacts: 'target/site/jacoco/**/*', allowEmptyArchive: true

          // Simple test result check
          script {
            def testResults = sh(script: 'find target/surefire-reports -name "*.xml" | wc -l', returnStdout: true).trim()
            echo "Found ${testResults} test result files"
          }
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
              -Dsonar.token=sqp_b109e7199c79e53fcd7e46677e1b0f1b4b694195
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