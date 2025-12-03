pipeline {
  agent { label 'windows' } // adjust label to your Windows node
  stages {
    stage('Checkout') {
      steps {
        checkout scm
        // optional: show where WORKSPACE points
        powershell 'Write-Host "WORKSPACE = $env:WORKSPACE"'
      }
    }

    stage('Build (container)') {
      steps {
        powershell '''
        Write-Host "Starting containerized build..."
        $projectSubfolder = Join-Path $env:WORKSPACE 'batch14-cicd'
        Write-Host "Project path: $projectSubfolder"

        if (-Not (Test-Path $projectSubfolder)) {
            Write-Error "Project folder not found: $projectSubfolder"
            exit 1
        }

        # canonicalize path and run docker with --mount (robust quoting)
        $hostFull = (Get-Item -LiteralPath $projectSubfolder).FullName
        Write-Host "Using host path: $hostFull"

        $image = 'node:18-alpine'
        $dockerArgs = "--rm --mount type=bind,source=`"$hostFull`",target=/workspace -w /workspace $image sh -c 'ls -la /workspace || true; if [ -f /workspace/package.json ]; then echo \"package.json exists\"; else echo \"NO package.json\"; fi; if [ -f /workspace/package.json ]; then npm install --legacy-peer-deps && npm run build; else exit 2; fi'"

        Write-Host "Running: docker $dockerArgs"
        # Run and stream output
        docker $dockerArgs
        '''
      }
    }
  }
  post {
    always {
      echo 'Done'
    }
  }
}
