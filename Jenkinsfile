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
        git url: 'git@github.com:cisnux-seed/automation-testing.git', branch: 'main'
      }
    }

    stage('Unit Test & Coverage') {
      steps {
        sh 'mvn package'
      }
      post {
        always {
          archiveArtifacts artifacts: 'target/surefire-reports/**/*', allowEmptyArchive: true

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
              -Dsonar.token=sqp_4c8dd0ee62f4b910399f65234103c266a0e1f0b6
        """
      }
    }
  }

  post {
    success {
      echo "Pipeline berhasil 🚀"
      echo "Hello World! This is a Jenkins pipeline for automation testing."
    }
    failure {
      echo "Pipeline gagal 💥"
    }
  }
}