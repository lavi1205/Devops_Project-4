pipeline{

    agent any

    
    environment{
        DOCKERHUB_USERNAME = "lavi1205"
        APP_NAME = "project3_thinh"
        IMAGE_TAG = "${BUILD_NUMBER}"
        IMAGE_NAME = "${DOCKERHUB_USERNAME}" + "/" + "${APP_NAME}"
        REGISTRY_CRED = 'dockerhub'
        NOMBRE_PIPELINE = "${env.JOB_NAME}"
        }
    
    stages{

        stage('check out git'){

            steps{
                script{
                    git branch: 'main', url: 'https://github.com/lavi1205/Devops_Project-4.git'
                }
            }
        }
        stage('Docker build and Push and Delete imgae'){
            steps{
                script{
                                        // def nomprePipeline = NOMBRE_PIPELINE.toLowerCase(); 
                                        // NOMBRE_PIPELINE = nomprePipeline
                                withCredentials([string(credentialsId: 'dockerhub_pass', variable: 'thinh')]) {
                                    sh '''
                                        docker login -u lavi1205 -p ${thinh}
                                        docker image build -t ${JOB_NAME}:v1.${BUILD_ID} .
                                        docker image tag ${JOB_NAME}:v1.${BUILD_ID} lavi1205/${JOB_NAME}:v1.${BUILD_ID}
                                        docker image tag ${JOB_NAME}:v1.${BUILD_ID} lavi1205/${JOB_NAME}:latest
                                        docker image push lavi1205/$JOB_NAME:v1.$BUILD_ID
                                        docker image push lavi1205/$JOB_NAME:latest
                                        docker rmi -f lavi1205/$JOB_NAME:v1.$BUILD_ID lavi1205/${JOB_NAME}:latest
                                    '''
                                }
                }
            } 
        }
        stage('Updating K8s Deployment file and push latest update to Github'){
            steps{
                script{

                    sh """
                        cat deployment.yml
                        sed -i 's/project3_thinh.*/project3_thinh:${IMAGE_TAG}/g' deployment.yml
                        cat deployment.yml
                        git config --global user.name "lavi1205"
                        git config --global user.mail "thinhphamthe874@gmail.com"
                        git add deployment.yml
                        git commit -m " Update Deployment File"
                    """
                        withCredentials([gitUsernamePassword(credentialsId: 'github', gitToolName: 'Default')]) {
                    sh "git push origin main"                     
                                }
                }
            }
        }
    }
}

            
            
