pipeline {
    agent {
        node {
            label 'maven1'
        }
    }
stages {
    stage("build"){
        steps {
            sh 'mvn clean deploy' 
        }
    }
    stage('SonarQube analysis') {
    environment {
      scannerHome = tool 'prashanth-sonar-scanner'
    }
    steps{
    withSonarQubeEnv('prashanth-sonar-server'){
            sh "${scannerHome}/bin/sonar-scanner"
        }
    }
    }
}
}
