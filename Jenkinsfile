pipeline {
  agent any
  stages {
    stage('Build And SonarQube analysis') {
      steps {
        script {
          // def scannerHome = tool 'ScannerForMSBuild'
          def SONARMSBUILD = '${HOME}/Applications/sonar-scanner-msbuild-5.5.3.43281-net5.0'
          def PATH = '${PATH}:${HOME}/.dotnet/tools:${HOME}/Applications/sonar-scanner-4.6.2.2472-macosx/bin/'
          def JAVA_HOME = '/Library/Java/JavaVirtualMachines/adoptopenjdk-11.jdk/Contents/Home'
          withSonarQubeEnv('SYCOLATEST') {
            sh "dotnet tool list -g" ;
            sh "dotnet --version" ;
            sh "dotnet ${SONARMSBUILD}/SonarScanner.MSBuild.dll begin /k:\"sylvain-combe-sonarsource_webapp\""
            sh "dotnet build"
            sh "dotnet ${SONARMSBUILD}/SonarScanner.MSBuild.dll end"
//          sh "dotnet tool install --global dotnet-sonarscanner || true" ;
//          sh "dotnet sonarscanner begin /k:\"sylvain-combe-sonarsource_webapp\" "
//          sh "dotnet build" ;
//          sh "dotnet sonarscanner end"
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
