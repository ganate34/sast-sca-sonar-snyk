pipeline {
  agent any
  tools { 
        maven 'Maven_3_5_2'  
    }
	
stages{
    stage('Build1'){
      steps{
        checkout changelog: false, poll: false, scm: scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/ganate34/sast-sca-sonar-snyk.git']])
      }
    }
    stage('Build2') {
      steps {
           // Get some code from a GitHub repository
           git 'https://github.com/ganate34/sast-sca-sonar-snyk.git'

          // Run Maven on a Unix agent.
          sh "mvn -Dmaven.test.failure.ignore=true clean package -fn"

          // To run Maven on a Windows agent, use
          // bat "mvn -Dmaven.test.failure.ignore=true clean package"
            }
        }
    stage('Pre-Test'){
        steps{
          sleep time: 5, unit: 'MILLISECONDS'
        }
      }
    stage('TestJavaVersion'){
      steps{
        sh 'java --version'
      }
    }
    stage('TestMavenVersion'){
      steps{
        sh 'mvn --version'
      }
    }
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
