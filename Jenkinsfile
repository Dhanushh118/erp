pipeline {
  agent any

  stages {
    stage('Checkout') {
      steps {
        git branch: 'main', url: 'https://github.com/Dhanushh118/erp'
      }
    }

    stage('Install / Test / Build (Windows agent)') {
      steps {
        // Use Windows cmd (bat) to invoke docker; the container runs sh internally.
        // Keep everything in one command to avoid shell mismatches.
        bat '''
          echo Starting containerized build...
          docker run --rm -v "%CD%":/workspace -w /workspace node:18-alpine ^
            sh -c "npm install --legacy-peer-deps && npm test -- --watchAll=false || true && npm run build"
        '''
      }
    }
  }
}
