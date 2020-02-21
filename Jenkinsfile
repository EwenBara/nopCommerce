pipeline {
  agent any
  stages {
    stage('Prepare') {
      steps {
        sh 'dotnet restore ./src/Build/src/ClearPluginAssemblies.sln'
        sh 'dotnet build ./src/Build/src/ClearPluginAssemblies.sln -c Release'
        sh 'dotnet publish ./src/Build/src/ClearPluginAssemblies.sln -c Release'
        sh 'cp -v ./src/Build/src/ClearPluginAssemblies/bin/Release/netcoreapp2.2/publish/ClearPluginAssemblies.dll  ./src/Build/ClearPluginAssemblies.dll'
      }
    }

    stage('Build') {
      steps {
        sh 'dotnet restore ./src/NopCommerce.sln'
        sh 'dotnet build -c Release ./src/NopCommerce.sln'
      }
    }

    stage('Unit Tests') {
      parallel {
        stage('Core Tests') {
          steps {
            catchError(buildResult: 'UNSTABLE', stageResult: 'FAILURE') {
              sh 'dotnet test ./src/Tests/Nop.Core.Tests/Nop.Core.Tests.csproj -c Release --logger trx --no-build'
            }
          }
        }

        stage('Web MVC Tests') {
          steps {
            catchError(buildResult: 'UNSTABLE', stageResult: 'FAILURE') {
              sh 'dotnet test ./src/Tests/Nop.Web.MVC.Tests/Nop.Web.MVC.Tests.csproj -c Release --logger trx --no-build'
            }
          }
        }

        stage('Services Tests') {
          steps {
            catchError(buildResult: 'UNSTABLE', stageResult: 'FAILURE') {
              sh 'dotnet test ./src/Tests/Nop.Services.Tests/Nop.Services.Tests.csproj -c Release --logger trx --no-build'
            }
          }
        }
      }
    }

    stage('Publish') {
      steps {
        sh 'dotnet publish src/Presentation/Nop.Web/Nop.Web.csproj -c Release --no-build'
        zip zipFile: 'nopCommerce.zip', archive: false, dir: 'src/Presentation/Nop.Web/bin/Release/netcoreapp2.2/publish/'
        archiveArtifacts artifacts: 'nopCommerce.zip', fingerprint: true
      }
    }

  }

  post {
    always {
      mstest testResultsFile:"**/*.trx", keepLongStdio: true
      deleteDir()
    }
  }
}