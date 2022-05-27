pipeline {
  agent any
  
  stages {
    stage ('Initialize') {
            steps {
                
                    echo "PATH = ${PATH}"
                    echo "M2_HOME = ${MAVEN_HOME}"
               
       }
    }
    stage('Unit Test') {
      steps {
	  bat 'mvn clean test'
        
      }
    }
    stage('Deploy CloudHub') {
      environment {
        ANYPOINT_CREDENTIALS = credentials('anypoint.credentials')
        muleEnv = "${env.cloudhub_env.toLowerCase()}"
      }
      steps {
        echo "----Publishing to Artifactory------ "
        bat 'mvn clean package deploy -U -Dmaven.test.skip=true'
        echo "----Running Build ${env.BUILD_ID} on muleEnv - ${muleEnv}----- "
        bat 'mvn clean package deploy -DmuleDeploy -P cloudhub -Danypoint.username=${ANYPOINT_CREDENTIALS_USR} -Danypoint.password=${ANYPOINT_CREDENTIALS_PSW} -Dmule.env=${muleEnv}'
      }
    }
  }
}
