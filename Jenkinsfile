// SIT223/753 - Task 7.1C - Part 1 Task 2: DevSecOps Basics
// Security testing of the intentionally vulnerable nodejs-goof project using npm.
// Aaryan Dandona (s224842524)
//
// Windows Jenkins: 'bat' is used instead of 'sh', and '|| exit /b 0' instead of '|| true'
// so that the pipeline continues past expected test/tooling failures.

pipeline {
  agent any

  // Poll GitHub every minute so a new commit triggers the pipeline automatically
  triggers {
    pollSCM('* * * * *')
  }

  stages {

    stage('Checkout') {
      steps {
        git branch: 'main', url: 'https://github.com/automacioai-code/8.2CDevSecOps.git'
      }
    }

    stage('Install Dependencies') {
      steps {
        echo 'Installing project dependencies with npm.'
        bat 'call npm install --legacy-peer-deps --no-audit --no-fund || exit /b 0'
      }
    }

    stage('Run Tests') {
      steps {
        echo 'Running the project test suite. Failures are tolerated so the security scan still runs.'
        bat 'call npm test || exit /b 0'
      }
    }

    stage('Generate Coverage Report') {
      steps {
        echo 'Generating a coverage report for later static analysis.'
        bat 'call npm run coverage || exit /b 0'
      }
    }

    stage('NPM Audit (Security Scan)') {
      steps {
        echo 'Running npm audit - this lists the known CVEs present in the dependency tree.'
        bat 'call npm audit || exit /b 0'
      }
    }
  }

  post {
    always {
      echo 'DevSecOps pipeline finished - review the NPM Audit stage output for the vulnerability report.'
    }
  }
}
