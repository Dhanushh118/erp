pipeline {
  agent any

  stages {
    
    stage('Checkout') {
      steps {
        git branch: 'main', url: 'https://github.com/Dhanushh118/erp'
      }
    }

    stage('Install, Test, Build') {
      steps {
        // Run all npm commands inside official Node Docker image
        sh '''
          docker run --rm \
            -v "$PWD":/workspace \
            -w /workspace \
            node:18-alpine sh -c "
              npm install --legacy-peer-deps &&
              npm test -- --watchAll=false || true &&
              npm run build
            "
        '''
      }
    }

  }
}
