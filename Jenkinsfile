pipeline {
        agent any
		//agent { docker { image 'node:19.3'} }

		environment {
			dockerHome = tool 'myDocker'
			mavenHome = tool 'myMaven'
			PATH ="$dockerHome/bin:$mavenHome/bin:$PATH"
		}
		stages { 
             stage('Checkout') {
				steps {
					sh 'mvn --version'
					sh 'docker version'
					echo "Build"
					echo "PATH - $PATH"
					echo "BUILD_NUMBER - $env.BUILD_NUMBER"
					echo "BUILD_ID - $env.BUILD_ID"
					echo "JOB_NAME - $env.JOB_NAME"
					echo "BUILD_TAG - $env.BUILD_TAG"
					echo "BUILD_URL - $env.BUILD_URL"
				}
			 }

			  stage ('Compile'){
				steps {
					sh "mvn clean compile"
				}

			  }

            stage('Test') {
				steps {
					sh "mvn test"
				}
			 }

			 stage('Integration Test') {
				steps {
					sh "mvn failsafe:integration-test failsafe:verify"
				}
			 }
			 stage('Build Docker Image'){
				steps{
					//"docker build -t in28min/currency-exchange-devops:$env.BUILD_TAG"
                    script {
						dockerImage = docker.build("in28min/currency-exchange-devops:${env.BUILD_TAG}")
					}
				}
			 }
			 stage('Push Docker'){
				steps{
					script{
						docker.withRegistry('','dockerhub')
						dockerImage.push();
						dockerImage.push('latest');
					}

				}
			 }

		}	  
		post {
			always {
				echo 'Im awesome. I run always'
			}
			success {
				echo 'I run when you are success'
			}
			failure {
				echo 'I run when you fail'
			}
		}
    }
