pipeline{
	agent any
	stages{
		stage('Build Backend'){
			steps{
				bat 'mvn clean package -DspikTests=true'
			}
		}
		stage('Unit Tests'){
			steps{
				bat 'mvn test'
			}
		}
		stage('Sonar Analysis'){
			environment{
				scannerHome= tool 'SONAR_SCANER'
			}
			steps{
				withSonarQubeEnv('SONAR_LOCAL'){
					bat "${scannerHome}/bin/sonar-scanner -e -Dsonar.projectKey=DeployBack -Dsonar.host.url=http://localhost:9000 -Dsonar.login=95ac43c573310416bad7105dbe54908bf38711da-Dsonar.java.binaries=target-Dsonar.coverage.exclusions= **/.mvn/**, **/src/test/**, **/model/**, **Application.java"
				}
			}
		}
	}
}
