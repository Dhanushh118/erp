pipeline {
  agent any

  stages {
    stage('Checkout') {
      steps { git branch: 'main', url: 'https://github.com/Dhanushh118/erp' }
    }

    stage('Install / Test / Build (Windows)') {
      steps {
        // Runs a Linux container on Windows host; container runs sh internally.
        // %CD% is current folder in cmd; ^ is line continuation.
        bat '''
          docker run --rm ^
            -v "%CD%":/workspace -w /workspace ^
            node:18-alpine sh -c "npm install --legacy-peer-deps && npm test -- --watchAll=false || true && npm run build"
        '''
      }
    }
  }
}
