pipeline {
  agent any

  stages {
    stage('Checkout') {
      steps { git branch: 'main', url: 'https://github.com/Dhanushh118/erp' }
    }

    stage('Install / Test / Build (Windows agent)') {
      steps {
        // Use Windows cmd to run docker; container runs Linux commands inside 'sh'
        bat '''
          REM Ensure %CD% maps project workspace; run a single docker container to handle npm tasks
          docker run --rm ^
            -v "%CD%":/workspace -w /workspace ^
            node:18-alpine sh -c "npm install --legacy-peer-deps && npm test -- --watchAll=false || true && npm run build"
        '''
      }
    }
  }
}
