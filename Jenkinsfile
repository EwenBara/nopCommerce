pipeline {
  agent any
  stages {
    stage('Build') {
      steps {
        sh 'dotnet clean build -c Release ./src/NopCommerce.sln'
      }
    }

    // stage('Unit test') {
    //   steps {
    //     dir(path: 'complete') {
    //       sh 'mvn test'
    //       sh 'mvn jacoco:report'
    //       junit 'target/surefire-reports/*.xml'
    //     }

    //   }
    // }

    // stage('Code Quality') {
    //   steps {
    //     dir(path: 'complete') {
    //       sh 'mvn sonar:sonar'
    //     }
    //   }
    // }

    // stage('Build Candidate') {
    //   parallel {
    //     stage('Package') {
    //       steps {
    //         dir(path: 'complete') {
    //           sh 'mvn -DskipTests package'
    //           archiveArtifacts 'target/**/*.jar'
    //         }

    //       }
    //     }

    //     stage('Version tag') {
    //       steps {
    //         sh('git tag ${BUILD_TIMESTAMP}BC')
    //         withCredentials([usernamePassword(credentialsId: '7c42fcf2-6fd7-408f-942b-bf0581980fca', passwordVariable: 'GIT_PASSWORD', usernameVariable: 'GIT_USERNAME')]) {
    //           sh 'git config --local credential.helper "!p() { echo username=\\$GIT_USERNAME; echo password=\\$GIT_PASSWORD; }; p"'
    //           sh('git push --tags')
    //         }
    //       }
    //     }

    //   }
    // }

  }
}