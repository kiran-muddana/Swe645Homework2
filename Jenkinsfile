
pipeline{
    agent any
    environment {
		DOCKERHUB_CREDENTIALS=credentials('dockerhub')
	}
  stages{
    stage('Build') {
      steps {
	sh 'rm -rf *.war'
        sh 'jar -cvf SurveyForm.war .'     
        sh 'docker build -t skm05/surveyform:${BUILD_NUMBER} .'
      }
    }
    stage('Login') {
      steps {
        sh 'echo $DOCKERHUB_CREDENTIALS_PSW |docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
       }
    }
    stage("Push image to docker hub"){
      steps {
        sh 'docker push skm05/surveyform:${BUILD_NUMBER}'
      }
    }
        stage("deploying on k8")
	{
		steps{
			sh 'kubectl set image deployment/homework2 container-0=skm05/surveyform:${BUILD_NUMBER} -n default'
			sh 'kubectl rollout restart deploy homework2 -n default'
		}
	} 
  }
 
  post {
	  always {
			sh 'docker logout'
		}
	}    
}
