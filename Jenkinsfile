properties([parameters([choice(choices:'true\nfalse',description:'Exclude Testcases',name:'tests')])])

pipeline {
    agent any 
    stages {
          stage ('Packaging Location') {
                sh '''
                    echo "PATH = ${PATH}"
                    echo "M2_HOME = ${M2_HOME}"
                '''
                sh "mvn clean -Dmaven.test.skip=${params.tests}"
        }    

        stage('killing containers') {
             try{
                
            sh '''
            docker kill demo-app-9090
            '''
            }
            catch(e){
                sh "echo no containers"
            }
            try{
                
            sh '''
            docker rm demo-app-9090
            '''
            }
            catch(e){
                sh "echo no containers"
            }
            
        } 

        stage ('Building demo-app image') {
                sh '''
                docker build -t demo-app . 
                '''
        }


stage ('Running demo-app container') {
                sh '''
                docker run  --name=demo-app-container-9090 --restart=always --net=demo-network -d -p 9090:9090 demo-app
                '''
}
    }
}