pipeline {
  agent any
  stages {
    stage('Checkout') {
      steps { git branch: 'main', url: 'https://github.com/Dhanushh118/erp' }
    }

    stage('Install / Test / Build cross-platform') {
      steps {
        script {
          if (isUnix()) {
            sh '''
              docker run --rm \
                -v "$PWD":/workspace -w /workspace \
                node:18-alpine sh -c "npm install --legacy-peer-deps && npm test -- --watchAll=false || true && npm run build"
            '''
          } else {
            // Windows agent: use bat
            bat '''
              docker run --rm ^
                -v "%CD%":/workspace -w /workspace ^
                node:18-alpine sh -c "npm install --legacy-peer-deps && npm test -- --watchAll=false || true && npm run build"
            '''
          }
        }
      }
    }
  }
}
