pipeline {
  agent any
  stages {
    stage('Build And SonarQube analysis') {
      steps {
        withSonarQubeEnv('sq3') {
          script {
            def scannerHome = tool 'SonarScanner for MSBuild'
            sh "dotnet ${scannerHome}/SonarScanner.MSBuild.dll begin /k:\"sylvain-combe-sonarsource_webapp\""
            sh "dotnet build"
            sh "dotnet ${scannerHome}/SonarScanner.MSBuild.dll end"
 //           sh "dotnet tool install --global dotnet-sonarscanner || true" ;
            def PATH = '${PATH}:${HOME}/.dotnet/tools:${HOME}/Applications/sonar-scanner-4.6.2.2472-macosx/bin/'
            def SONARMSBUILD = '${HOME}/Applications/sonar-scanner-msbuild-5.5.3.43281-net5.0'
            def JAVA_HOME = '/Library/Java/JavaVirtualMachines/adoptopenjdk-11.jdk/Contents/Home'
            sh "dotnet tool list -g" ;
            sh "dotnet --version" ;
            // sh "dotnet sonarscanner begin /k:\"sylvain-combe-sonarsource_webapp\" "
//          sh "dotnet ${SONARMSBUILD}/SonarScanner.MSBuild.dll begin /k:\"sylvain-combe-sonarsource_webapp\" " ;
//          sh "dotnet build" ;
//          sh "dotnet ${SONARMSBUILD}/SonarScanner.MSBuild.dll end"
            // sh "dotnet sonarscanner end"
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
