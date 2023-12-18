pipeline {
  agent any
  tools { 
        maven 'Maven_3_6_3'  
    }
   stages{
    stage('CompileandRunSonarAnalysis') {
            steps {	
		sh 'mvn clean verify sonar:sonar -Dsonar.projectKey=ganate34github -Dsonar.organization=ganate34github -Dsonar.host.url=https://sonarcloud.io -Dsonar.login=88cb0a5fb57556c50b136668178348f51c780530'
			}
        } 
     stage('RunSCAAnalysisUsingSnyk') {
            steps {		
		withCredentials([string(credentialsId: 'SNYK_TOKEN', variable: 'SNYK_TOKEN')]) {
			sh 'mvn snyk:test -fn'
				}
			}
    }		
  }
}
