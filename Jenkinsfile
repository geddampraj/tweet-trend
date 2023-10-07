pipeline {
    agent {
        node {
            label 'maven1'
        }
    }
}
stages {
    stage("build"){
        steps {
            sh 'mvn clean deploy' 
        }
    }
}
}
