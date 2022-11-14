pipeline{
    //Directives
    agent any
    tools {
        maven 'maven-3'
    }

    stages {
        // Specify various stage with in stages

        // stage 1. Git clone
        
        stage ('Git Clone'){
            steps {
                git credentialsId: 'GitHubCred', url: 'https://github.com/28sanjayrp/my-app.git'
                //git branch: 'master', url: '', credentials: ''
            }
        }
        
         // stage 2. war files creation
        
        stage ('Maven-build-creating war files'){
            steps {
                sh 'mvn clean compile package install'
            }
        }

        // Stage 3 : Docker build and Tag
        
        stage ('Docker build and Tag'){
            steps {
                sh 'docker build -t sampleapp:latest .'    //build
                sh 'docker tag sampleapp sanjayrp/sampleapp:latest'  //tagging
                //sh 'docker tag sampleapp sanjayrp/sampleapp:$BUILD_NUMBER'


            }
        }
        
        // stage 4. publish or pushing image to docker hub
        
        stage ('Image pushing'){
            steps {
                withDockerRegistry([ credentialsId: " ", url: " " ]) {
          sh  'docker push sanjayrp/sampleapp:latest'
          sh  'docker push sanjayrp/sampleapp:$BUILD_NUMBER' 
            }
        }
            
            
         // stage 5.  port mapping 
        
        stage ('port-mapping'){
            steps {
                sh 'docker run -it -d -p 8888:8080 sanjayrp/sampleapp'
            }
        }
        
             // stage 6.  Run docker conatainer(tomcat) on host server
        
        stage ('running conatiner'){
            steps {
                sh 'docker -H ssh//jenkins@publicIPofhost run -it -d -p 8888:8080 sanjayrp/sampleapp'
            }
        }
            
            
            // stage 7. email notification
            post {
        always {
            emailext body: 'A Test EMail', recipientProviders: [[$class: 'DevelopersRecipientProvider'], [$class: 'RequesterRecipientProvider']], subject: 'Test'
        }
    }

}
