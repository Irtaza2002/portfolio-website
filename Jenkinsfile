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
    post{
        success{
            script{
                emailext from : 'iratzajaved31@gmail.com',
                to : 'irtazajaved31@gmail.com',
                subject : 'pipeline successfull',
                body : 'CI/CD pipeline successfully executed'
            }
        }
    }
}
