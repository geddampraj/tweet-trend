pipeline {
    agent {
        node {
            label 'maven1'
        }
    }
stages {
    stage("build"){
        steps {
            echo "----------- build started ----------"
            sh 'mvn clean deploy -Dmaven.test.skip=true'
            echo "----------- build complted ----------"
        }
    }
 stage("test"){
            steps{
                echo "----------- unit test started ----------"
                sh 'mvn surefire-report:report'
                 echo "----------- unit test Complted ----------"
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
    stage("Quality Gate") {
        steps {
            script {
                timeout(time: 1, unit: 'HOURS')
         def qg = waitForQualityGate()
         if (qg.status != 'OK') {
            error "pipeline aborted due to quality gate failure: ${qg.status}"
                }
            }
        }
        }
}
}
