pipeline {
  agent any
  stages {
    stage('Build') {
      steps {
        sh 'mvn clean install'
      }
    }
    stage('Deploy to Nexus') {
      steps {
        sh 'mvn deploy'
      }
    }
    stage('Deploy to Tomcat') {
      steps {
        sh 'cp target/viteapp-web.war /opt/tomcat/webapps/ROOT.war'
      }
    }
  }
}
