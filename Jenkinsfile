pipeline {
   agent any

   stages {
      stage('pull code') {
          steps {
              checkout([$class: 'GitSCM', branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[credentialsId: 'endeavor', url: 'git@github.com:endeavor66/web_demo.git']]])
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
      stage('deploy project') {
          steps {
              deploy adapters: [tomcat8(credentialsId: '18ee569a-7f0c-408b-a4d4-4675dd891bc1', path: '', url: 'http://192.168.174.100:8080/')], contextPath: null, war: 'target/*.war'
          }
      }
   }
}