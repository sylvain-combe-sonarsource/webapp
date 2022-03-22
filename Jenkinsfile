pipeline {
  agent any
  stages {
    stage('Build And SonarQube analysis') {
      steps {
        withSonarQubeEnv('sq3') {
         script {
            def scannerHome = tool 'ScannerForMSBuild'
            // sh "dotnet ${scannerHome}/SonarScanner.MSBuild.dll begin /k:\"webapp\" " ;
            // sh "dotnet build" ;
            // sh "dotnet ${scannerHome}/SonarScanner.MSBuild.dll end"
            // dotnet tool install --global dotnet-sonarscanner
            sh "dotnet sonarscanner begin /k:\"webapp\""
            sh "dotnet build <path to solution.sln>"
            sh "dotnet sonarscanner end"
	  }
        }
      }
    }

    stage("Quality Gate") {
      steps {
        timeout(time: 1, unit: 'HOURS') {
          // Parameter indicates whether to set pipeline to UNSTABLE if Quality Gate fails
          // true = set pipeline to UNSTABLE, false = don't
          waitForQualityGate abortPipeline: true
        }
      }
    }
  }
}
