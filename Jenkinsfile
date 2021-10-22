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
				scannerHome= tool 'SONAR_SCANNER'
			}
			steps{
				withSonarQubeEnv('SONAR_LOCAL'){
					bat "${scannerhome}/sonar-scanner-4.6.2.2472/bin/sonar-scanner -Dsonar.projectKey=DeployBack -Dsonar.host.url=http://localhost:9000 -Dsonar.login=95ac43c573310416bad7105dbe54908bf38711da -Dsonar.java.binaries=target -Dsonar.coverage.exclusions=**/.mvn/**,**/src/test/**,**/model/**,**Application.java"
				}
			}
		}
		stage('Quality Gate'){
			steps{
				sleep(5)
				timeout(time:1, unit:'MINUTES'){
					waitForQualityGate abortPipeline: true
				}
			}
		}
		stage('Deploy Backend'){
			steps{
				deploy adapters: [tomcat8(credentialsId: 'TomcatLogin', path: '', url: 'http://localhost:8001/')], contextPath: 'tasks-backend', war: 'target/tasks-backend.war'
			}
		}
		stage('API Test'){
			steps{
				dir('api-test') {
					git credentialsId: 'github_login', url: 'https://github.com/Antoniojustino4/tasks-api-test'
					bat 'mvn test'
				}
			}
		}
	}
}
