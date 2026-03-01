pipeline {
    agent { label "dev" }

    stages {
        stage('code') {
            steps {
                git url: "https://github.com/kiranpatil22/two-tier-flask-app.git", branch: "master"
            }
        }

        stage('Build') {
            steps {
             sh "docker build --no-cache -t two-tier-flask-app ."
            }
        }

        stage('Test') {
            steps {
                sh 'echo Running tests...'
            }
        }
        stage('push to docker') {
            steps{
                withCredentials([usernamePassword(
                    credentialsId:"dockerHubCreds",
                    passwordVariable: "dockerHubPass" ,
                 usernameVariable: "dockerHubUser"
                 )])        
              {
                sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                sh "docker image tag two-tier-flask-app ${env.dockerHubUser}/two-tier-flask-app"
                sh "docker push ${env.dockerHubUser}/two-tier-flask-app:latest"
             }
                 }
        }
        stage("Deploy"){
            steps { 
                sh "docker compose up -d --build flask-app"
                  }
             }
        stage("Cleanup"){
    steps{
        sh "docker image prune -f"
    }
}
        }
post {
    success{
        emailext(
            from: 'kiranpatiluniverse@gmail.com',
            subject: "Build Successful",
            body: "Good news: Your build  successful",
            to: 'kiranpatiluniverse@gmail.com')
    }
    failure{
         emailext(
            from: 'kiranpatiluniverse@gmail.com',
            subject: "Build failed",
            body: "Good news: Your build failed",
            to: 'kiranpatiluniverse@gmail.com')
        
    }
}
    }
