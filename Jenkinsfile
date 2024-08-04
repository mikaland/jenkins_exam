/* groovylint-disable CompileStatic */
pipeline {
    environment { // Declaration of environment variables
        DOCKER_ID = 'mikalander77' // replace this with your docker-id
        DOCKER_IMAGE_MOVIE = 'jenkins_exam_movie'
        DOCKER_IMAGE_CAST = 'jenkins_exam_cast'
        DOCKER_TAG = "v.${BUILD_ID}.0"
    // we will tag our images with the current build in order to increment the value by 1 with each new build
    }
    agent any // Jenkins will be able to select all available agents
    stages {
        stage('docker:build:movie') { // docker build image stage
            steps {
                script {
                    sh '''
                    cd movie-service
                    docker rm -f movie
                    docker build -t $DOCKER_ID/$DOCKER_IMAGE_MOVIE:$DOCKER_TAG .
                    sleep 6
                    '''
                }
            }
        }
        stage('docker:run:movie') { // run container from our builded image
            steps {
                script {
                    sh '''
                    docker run -d -p 8001:80 --name movie $DOCKER_ID/$DOCKER_IMAGE_MOVIE:$DOCKER_TAG
                    sleep 10
                    '''
                }
            }
        }
        stage('test:acceptance:movie') {
            // we launch the curl command to validate that the container responds to the request
            steps {
                script {
                    sh '''
                    curl localhost:8001
                    '''
                }
            }
        }
        stage('docker:push:movie') { //we pass the built image to our docker hub account
            environment {
                DOCKER_PASS = credentials('DOCKER_HUB_PASS')
            // we retrieve  docker password from secret text called docker_hub_pass saved on jenkins
            }
            steps {
                script {
                    sh '''
                    docker login -u $DOCKER_ID -p $DOCKER_PASS
                    docker push $DOCKER_ID/$DOCKER_IMAGE_MOVIE:$DOCKER_TAG
                    '''
                }
            }
        }
        stage('docker:build:cast') { // docker build image stage
            steps {
                script {
                    sh '''
                    docker rm -f cast
                    docker build -t $DOCKER_ID/$DOCKER_IMAGE_CAST:$DOCKER_TAG .
                    sleep 6
                    '''
                }
            }
        }
        stage('docker:run:cast') { // run container from our builded image
            steps {
                script {
                    sh '''
                    docker run -d -p 8002:80 --name cast $DOCKER_ID/$DOCKER_IMAGE_CAST:$DOCKER_TAG
                    sleep 10
                    '''
                }
            }
        }
        stage('test:acceptance:cast') {
            // we launch the curl command to validate that the container responds to the request
            steps {
                script {
                    sh '''
                    curl localhost:8002
                    '''
                }
            }
        }
        stage('docker:push:cast') { //we pass the built image to our docker hub account
            environment {
                DOCKER_PASS = credentials('DOCKER_HUB_PASS')
            // we retrieve  docker password from secret text called docker_hub_pass saved on jenkins
            }
            steps {
                script {
                    sh '''
                    docker login -u $DOCKER_ID -p $DOCKER_PASS
                    docker push $DOCKER_ID/$DOCKER_IMAGE_CAST:$DOCKER_TAG
                    '''
                }
            }
        }
        stage('deploy:dev') {
            environment {
                KUBECONFIG = credentials('config')
            // we retrieve  kubeconfig from secret file called config saved on jenkins
            }
            steps {
                script {
                    sh '''
                    rm -Rf .kube
                    mkdir .kube
                    ls
                    cat $KUBECONFIG > .kube/config
                    cp k8s/chart/values.yaml values.yml
                    cat values.yml
                    sed -i "s+tag.*+tag: ${DOCKER_TAG}+g" values.yml
                    helm upgrade --install app k8s/chart --values=values.yml --namespace dev
                    '''
                }
            }
        }
        stage('deploy:staging') {
            environment {
                KUBECONFIG = credentials('config')
            // we retrieve  kubeconfig from secret file called config saved on jenkins
            }
            steps {
                script {
                    sh '''
                    rm -Rf .kube
                    mkdir .kube
                    ls
                    cat $KUBECONFIG > .kube/config
                    cp k8s/chart/values.yaml values.yml
                    cat values.yml
                    sed -i "s+tag.*+tag: ${DOCKER_TAG}+g" values.yml
                    helm upgrade --install app k8s/chart --values=values.yml --namespace staging
                    '''
                }
            }
        }
        stage('deploy:prod') {
            environment {
                KUBECONFIG = credentials('config')
                // we retrieve  kubeconfig from secret file called config saved on jenkins
            }
            steps {
                // Create an Approval Button with a timeout of 15minutes.
                // this require a manuel validation in order to deploy on production environment
                timeout(time: 15, unit: 'MINUTES') {
                    input message: 'Do you want to deploy in production ?', ok: 'Yes'
                }
                script {
                    sh '''
                    rm -Rf .kube
                    mkdir .kube
                    ls
                    cat $KUBECONFIG > .kube/config
                    cp k8s/chart/values.yaml values.yml
                    cat values.yml
                    sed -i "s+tag.*+tag: ${DOCKER_TAG}+g" values.yml
                    helm upgrade --install app k8s/chart --values=values.yml --namespace prod
                    '''
                }
            }
        }
    }
    post {
        // send email when the job has failed
        failure {
            echo 'This will run if the job failed'
            mail to: 'mickaelkremer@gmail.com',
            subject: "${env.JOB_NAME} - Build # ${env.BUILD_ID} has failed",
            body: "For more info on the pipeline failure, check out the console output at ${env.BUILD_URL}"
        }
    }
}
