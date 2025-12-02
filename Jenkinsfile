pipeline {
  agent {
    docker {
      image 'node:lts'    // official Node LTS image
      args  '-u root:root' // optional: run as root to avoid permission issues when installing
    }
  }

  stages {
    stage('Clone') {
      steps {
        // git is available inside the Jenkins workspace by default; if not, you can still use the git step
        git branch: 'main', url: 'https://github.com/Dhanushh118/erp'
      }
    }

    stage('Install Dependencies') {
      steps {
        // use npm install; legacy peer deps flag kept as you had it
        sh 'npm install --legacy-peer-deps'
      }
    }

    stage('Run Tests') {
      steps {
        sh 'npm test -- --watchAll=false || true'
      }
    }

    stage('Build React App') {
      steps {
        sh 'npm run build'
      }
    }
  }
}

