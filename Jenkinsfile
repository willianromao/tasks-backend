pipeline {
	agent any
	stages {
		stage ('tasks-backend: Build') {
			steps {
				bat 'mvn clean package -DskipTests=true'
			}
		}
		stage ('tasks-backend: Unit Tests') {
			steps {
				bat 'mvn test'
			}
		}
		stage ('tasks-backend: Sonar Analysis') {
			environment {
				scannerHome = tool 'SONAR_SCANNER'
			}
			steps {
				withSonarQubeEnv('SONAR_REMOTE') {
					bat "${scannerHome}/bin/sonar-scanner -e -Dsonar.projectKey=tasks-backend -Dsonar.host.url=http://52.250.57.59:9000 -Dsonar.login=f6ddd287d8e614d2856116cd494bba0ef62dfdaf -Dsonar.java.binaries=target"
				}
			}
		}
		stage ('tasks-backend: Quality Gate') {
			steps {
				sleep(5)
				timeout(time: 1, unit: 'MINUTES') {
					waitForQualityGate abortPipeline: true
				}
			}
		}
	}
}