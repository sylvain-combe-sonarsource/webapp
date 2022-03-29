pipeline {
  agent any
  stages {
    stage('Build And SonarQube analysis') {
      steps {
       withSonarQubeEnv('sq3') {
         script {
            sh "dotnet tool install --global dotnet-sonarscanner || true" ;
            def PATH = '${PATH}:${HOME}/.dotnet/tools'
            def SONARMSBUILD = '${HOME}/Applications/sonar-scanner-msbuild-5.5.3.43281-net5.0'
            sh "dotnet tool list -g" ;
            sh "dotnet --version" ;
            sh "dotnet sonarscanner begin /k:\"sylvain-combe-sonarsource_webapp\" "
            // sh "dotnet ${SONARMSBUILD}/SonarScanner.MSBuild.dll begin /k:\"sylvain-combe-sonarsource_webapp\" " ;
            sh "dotnet build" ;
            // sh "dotnet ${SONARMSBUILD}/SonarScanner.MSBuild.dll end"
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
