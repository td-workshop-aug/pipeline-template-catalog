library 'pipeline-library'
pipeline {
  agent none
  options {
    timeout(time: 10, unit: 'MINUTES')
    buildDiscarder(logRotator(numToKeepStr: '2'))
  }
  stages {
    stage("Import Catalog") {
      agent { label 'default-jnlp' } 
      when {
        branch 'main'
      }
      steps {
        withCredentials([usernamePassword(credentialsId: 'admin-cli-token', usernameVariable: 'JENKINS_CLI_USR', passwordVariable: 'JENKINS_CLI_PSW')]) {
          sh """
            curl -O http://td-workshop-aug-sakshi-td-controller/td-workshop-aug-sakshi-td-controller/jnlpJars/jenkins-cli.jar
            alias cli='java -jar jenkins-cli.jar -s http://td-workshop-aug-sakshi-td-controller/td-workshop-aug-sakshi-td-controller/ -auth $JENKINS_CLI_USR:$JENKINS_CLI_PSW'
            cli pipeline-template-catalogs --put < create-pipeline-template-catalog.json
          """
          pipelineCatalogLabCleanup('td-workshop-aug')
        }
      }
    }
  }
}
