pipeline {
  agent any
  stages {
    stage('Checkout') {
      steps { checkout scm }
    }
    stage('Build & Test') {
      steps {
        sh 'mvn -B clean install'
      }
      // JUnit reporting removed since no tests exist
    }
    stage('Run Sample') {
      steps {
        sh 'java -jar target/javaparser-maven-sample-1.0-SNAPSHOT.jar || true'
      }
    }
    stage('Archive') {
      steps { archiveArtifacts artifacts: 'target/*.jar', fingerprint: true }
    }
  }
}
