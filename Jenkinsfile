// properties([parameters([choice(choices:'true\nfalse',description:'Exclude Testcases',name:'tests')])])

pipeline {
    agent any 
    

    tools {
        maven 'M3'
    }

    
    environment {
        ORG_NAME = "naveen"
        APP_NAME = "demo-app"
        APP_VERSION = "1.0-SNAPSHOT"
        APP_CONTEXT_ROOT = "/"
        APP_LISTENING_PORT = "9090"
        TEST_CONTAINER_NAME = "ci-${APP_NAME}-${BUILD_NUMBER}"
        PREV_CONTAINER_NAME="ci-${APP_NAME}-${currentBuild.previousBuild.number}"
    }

  stages {
    stage('Compile') {
        steps {
            echo "-=- compiling project -=-"
            sh "mvn clean compile"
        }
    }
  

    stage('Package') {
        steps {
            echo "-=- packaging project -=-"
            sh "mvn package -DskipTests"
            archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
        }
    }

    stage ('Killing Containers') {
        steps {

            echo "====== killing Containers====="
            echo "Current Container: ${TEST_CONTAINER_NAME} "
            echo "Previous Container ${PREV_CONTAINER_NAME}"
            script {
                try {
                    sh "docker kill  ${TEST_CONTAINER_NAME}"
                }catch(e) {
                    echo "Container not found ${TEST_CONTAINER_NAME}"
                }
            }
        }
    }



    stage('Build Docker image') {
        steps {
            echo "-=- build Docker image -=-"
            sh "docker build -t ${ORG_NAME}/${APP_NAME}:${APP_VERSION} -t ${ORG_NAME}/${APP_NAME}:latest ."
        }
    }

   stage('Run Docker image') {
            steps {
                echo "-=- run Docker image -=-"
                sh "docker run --name ${TEST_CONTAINER_NAME} --detach  --network demo-network -p 9090:9090' ${ORG_NAME}/${APP_NAME}:latest"
            }
        }

  }



}