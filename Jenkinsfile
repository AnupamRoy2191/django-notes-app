pipeline {
    agent { label "test" }
	environment {
		DOCKERHUB_CREDS=credentials('dockerHub')
	}
    stages{
        stage("Clone Code"){
            steps{
                git url: "https://github.com/AnupamRoy2191/django-notes-app.git", branch: "main"
            }
        }
        stage("Build"){
            steps{
                sh "docker build . -t $DOCKERHUB_CREDS_USR/my-note-app:latest"
           	 }
        }
        stage("Push to Docker Hub"){
            steps{
                sh "docker login -u $DOCKERHUB_CREDS_USR -p '$DOCKERHUB_CREDS_PSW'"
                sh "docker push $DOCKERHUB_CREDS_USR/my-note-app:latest"
                }
            }
        stage("Deploy"){
            steps{
                sh "docker-compose down && docker-compose up -d"
            }
        }
    }
    post{
  	always {
    		echo "cleaning up all dangling images"
		sh 'docker image prune -f'
  }
}

}
