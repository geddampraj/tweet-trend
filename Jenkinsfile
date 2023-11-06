def registry = 'https://prashanth02.jfrog.io'
def imageName = 'prashanth02.jfrog.io/prashanth-docker-local/ttrend'
def version   = '2.1.2'
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
    stage("Quality Gate"){
    steps {
        script {
        timeout(time: 1, unit: 'HOURS') { // Just in case something goes wrong, pipeline will be killed after a timeout
    def qg = waitForQualityGate() // Reuse taskId previously collected by withSonarQubeEnv
    if (qg.status != 'OK') {
      error "Pipeline aborted due to quality gate failure: ${qg.status}"
    }
  }
}
    }
  }
    stage(" Docker Build ") {
      steps {
        script {
           echo '<--------------- Docker Build Started --------------->'
           app = docker.build(imageName+":"+version)
           echo '<--------------- Docker Build Ends --------------->'
        }
      }
    }

            stage (" Docker Publish "){
        steps {
            script {
               echo '<--------------- Docker Publish Started --------------->'  
                docker.withRegistry(registry, 'artifact-cred'){
                    app.push()
                }    
               echo '<--------------- Docker Publish Ended --------------->'  
            }
        }
    }
    stage ("Deploy"){
       steps {
          script {
              sh './deploy.sh'
          }
       }
    }
}
}
