pipeline {
  agent { label 'windows' }
  stages {
    stage('Checkout') {
      steps {
        checkout scm
        bat 'echo WORKSPACE=%WORKSPACE%'
      }
    }
    stage('Build (container)') {
      steps {
        bat '''
        echo Project folder: %WORKSPACE%\\batch14-cicd
        if not exist "%WORKSPACE%\\batch14-cicd" (
          echo ERROR: project folder missing
          exit /b 1
        )
        REM Use forward slashes for docker -v to avoid quoting issues
        docker run --rm -v "%WORKSPACE%\\batch14-cicd:/workspace" -w /workspace node:18-alpine sh -c "ls -la /workspace || true; if [ -f /workspace/package.json ]; then echo package.json exists; npm install --legacy-peer-deps && npm run build; else echo NO package.json; exit 2; fi"
        '''
      }
    }
  }
}
