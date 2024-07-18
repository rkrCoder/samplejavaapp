pipeline {
    agent any
    stages {
        stage('compile') {
	   steps {
                echo 'compiling..'
		git 'https://github.com/rkrCoder/samplejavaapp.git'
		sh script: '/opt/maven/bin/mvn compile'
           }
        }
        stage('codereview-pmd') {
	   steps {
                echo 'codereview..'
		sh script: '/opt/maven/bin/mvn -P metrics pmd:pmd'
           }
	   post {
               success {
		   recordIssues enabledForFailure: true, tool: pmdParser(pattern: '**/target/pmd.xml')
               }
           }		
        }
        stage('unit-test') {
	   steps {
                echo 'unittest..'
	        sh script: '/opt/maven/bin/mvn test'
                 }
	   post {
               success {
                   junit 'target/surefire-reports/*.xml'
               }
           }			
        }
        stage('codecoverage') {
	   steps {
              echo 'Running code coverage..'
         script {
            def mvnExitCode = sh script: '/opt/maven/bin/mvn verify', returnStatus: true
            if (mvnExitCode == 0) {
                currentBuild.result = 'SUCCESS'
            } else {
                currentBuild.result = 'FAILURE'
            }
        }
           }		
        }
        stage('package') {
	   steps {
                echo 'package......'
		sh script: '/opt/apache-maven-3.8.4/bin/mvn package'	
           }		
        }
    }
}
