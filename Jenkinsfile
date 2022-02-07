def git-auth="faaf8dd2-ff5c-4588-aca5-b0cd56df51de"

pipeline {
   stages {
      stage('pull code') {
          steps {
              checkout([$class: 'GitSCM', branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[credentialsId: "${git-auth}", url: 'git@github.com:endeavor66/web_demo.git']]])
          }
      }
      stage('code checking') {
          steps {
              script {
                  //引入SonarQubeScanner工具
                  scannerHome = tool 'sonarqube-scanner'
              }
              //引入SonarQube的服务器环境
              withSonarQubeEnv('sonarqube') {
                  sh "${scannerHome}/bin/sonar-scanner"
              }
          }
      }
      stage('build project') {
          steps {
              sh 'mvn clean package'
          }
      }
   }
}