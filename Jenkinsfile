pipeline{
    agent {label "dev"};
    
    stages{
        stage("Code Clone"){
            steps{
                git url: "https://github.com/Irtaza2002/portfolio-website.git"
            }
        }
        stage("Code Build"){
            steps{
                sh "docker build -t portfolio-website ."
            }
        }
        stage("Code Test"){
            steps{
                echo "Testing under progress..."
            }
        }
        stage("Push to DockerHub"){
            steps{
                withCredentials([usernamePassword(
                    credentialsId: "DockerHubCreds",
                    usernameVariable: "dockerHubUser",
                    passwordVariable: "dockerHubPass"
                    )]){
                        sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                        sh "docker image tag portfolio-website ${env.dockerHubUser}/portfolio-website"
                        sh "docker push ${env.dockerHubUser}/portfolio-website"
                    }
            }
        }
        stage("Code Deploy"){
            steps{
                sh "docker compose up -d --build"
            }
        }
    }
    post {
    success {
        emailext(
            to: 'irtazajaved31@gmail.com',
            subject: "Build Success: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
            body: """
Build succeeded.

Job: ${env.JOB_NAME}
Build Number: ${env.BUILD_NUMBER}
URL: ${env.BUILD_URL}
"""
        )
    }

    failure {
        emailext(
            to: 'irtazajaved31@gmail.com',
            subject: "Build Failed: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
            body: "Check Jenkins console output."
        )
    }
}
}
