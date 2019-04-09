pipeline {
  agent any
  stages {
    stage('build') {
      steps {
        sh './gradlew clean assemble'
      }
    }
    stage('Unit Test') {
      steps {
        sh './gradlew test'
      }
      post {
        always {
          junit 'build/test-results/test/*.xml'
        }
      }
    }
    stage('DB Tests') {
      steps {
        sh './gradlew fastIntegrationTest'
      }
	post {
	   always {
              junit 'build/test-results/fastIntegrationTest/*.xml'
           }
        }
    }
    stage('Dependency Scan') {
      parallel {
        stage('Dependency Scan') {
          steps {
            echo 'Scan dependencies'
            sleep 5
          }
        }
        stage('Static Security Scan') {
          steps {
            echo 'Run SAST Scan'
            sleep 5
          }
        }
      }
    }
    stage('Deploy to Test') {
      steps {
        echo 'Deploy to INT'
        sleep 5
      }
    }
    stage('Smoke Test') {
      steps {
        echo 'Run post-deployment smoke test'
        sleep 5
      }
    }
    stage('Functional Test') {
      parallel {
        stage('Functional Test') {
          steps {
            echo 'Run Automated Test Suite'
            sleep 5
          }
        }
        stage('Dynamic Security Testing') {
          steps {
            echo 'Run DAST Scan'
            sleep 5
          }
        }
      }
    }
    stage('Deploy to Stage') {
      steps {
        echo 'Run deployment to Stage environment'
      }
    }
    stage('Post Deploy Test') {
      steps {
        echo 'Run post-deployment tests in Stage'
      }
    }
  }
}
