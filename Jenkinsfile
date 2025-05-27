pipeline{
    agent any
    stages{
        stage('Clone'){
            steps{
                git branch:'main', url: 'https://github.com/ThakurSahilSingh/java-app-cicd.git'
                sh '''  
                echo "Repo Cloned"
                '''
            } 
        }
        //stage('Adding files'){
        //    steps{
        //        sh '''
        //        cp /root/java/java-app-cicd/Dockerfile /root/.jenkins/workspace/java-app/
        //        cp -r /root/java/java-app-cicd/k8s /root/.jenkins/workspace/java-app/
        //       '''
        //    }
        }
        stage('Build'){
            steps{
                withCredentials([usernamePassword(credentialsId: 'docker-cred', passwordVariable: 'docker-pass', usernameVariable: 'docker-user')]) {
                    sh '''
                    docker login -u "$docker-user" -p "$docker-pass"
                    docker build -t sahil0824/spring-boot-app:latest .
                    '''
                }
                
            }
        }
        stage('Push'){
            steps{
                withCredentials([usernamePassword(credentialsId: 'docker-cred', passwordVariable: 'docker-pass', usernameVariable: 'docker-user')]) {
                    sh '''
                    docker login -u "$docker-user" -p "$docker-pass"
                    docker push sahil0824/spring-boot-app:latest
                    '''
                }    
            }
        }
        stage('Deploy'){
            steps{
                withCredentials([file(credentialsId: 'k8-cred', variable: 'KUBECONFIG')]){
                    sh '''
                    kubectl apply -f k8s/. --namespace=default
                    '''
                }
            }
        }

    }
}
