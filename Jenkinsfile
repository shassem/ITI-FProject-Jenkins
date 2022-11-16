pipeline {
    agent any

    stages {

        stage('CI'){
            steps {

                withCredentials([usernamePassword(credentialsId: 'Dockerhub', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')])            {

                sh """
                    docker build -t shassem/hellopython:v$BUILD_NUMBER .
                    docker login -u ${USERNAME} -p ${PASSWORD}
                    docker push shassem/hellopython:v$BUILD_NUMBER

                """
                }
              }
        }
    

        stage('CD'){
            steps {

                withCredentials([file(credentialsId: 'AccessCluster', variable: 'config')]){
                    sh """
                        gcloud auth activate-service-account --key-file=${config}
                        gcloud container clusters get-credentials my-gke-cluster --region us-central1 --project neat-talent-367811
                        sed -i 's/tag/${env.BUILD_NUMBER}/g' python.yml
                        kubectl apply -f python.yml
                    """
                }
            }
 
        }
    }    
}



