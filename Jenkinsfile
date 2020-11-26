// properties([parameters([choice(choices:'true\nfalse',description:'Exclude Testcases',name:'tests')])])

pipeline {
    agent any 
    

    tools {
        maven 'M3'
    }

    
    environment {
        ORG_NAME = "Naveen"
        APP_NAME = "demo-app"
        APP_VERSION = "1.0-SNAPSHOT"
        APP_CONTEXT_ROOT = "/"
        APP_LISTENING_PORT = "9090"
        TEST_CONTAINER_NAME = "ci-${APP_NAME}-${BUILD_NUMBER}"
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


        stage('Build Docker image') {
            steps {
                echo "-=- build Docker image -=-"
                sh "docker build -t ${ORG_NAME}/${APP_NAME}:${APP_VERSION} -t ${ORG_NAME}/${APP_NAME}:latest ."
            }
        }

  }



}