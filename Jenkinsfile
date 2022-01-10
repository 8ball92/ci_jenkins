pipeline {
    agent any
    environment {
		DOCKERHUB_CREDENTIALS=credentials('dockerhub')
	}
    stages {
        stage('Build mvn packege') {
            steps {
                git branch: "${env.BRANCH}",
                url: 'https://github.com/8ball92/maven-hello-world.git'
                sh "cd my-app && mvn package " 
                  
            }
        }
        stage('Building image') {
            steps {
                sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
                sh "docker build -t gblbjj/${env.APP}:${env.TAG_VERSION} ."
                sh "docker push gblbjj/${env.APP}:${env.TAG_VERSION}"    
            }
        }

    }
    post {
        always {
            script {
                echo "THE END JOB"
                sh "docker rmi gblbjj/${env.APP}:${env.TAG_VERSION}"
                
            }
            
            cleanWs deleteDirs: true, patterns: [[pattern: '', type: 'EXCLUDE']]
        }
    }        
}
