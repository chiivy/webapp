pipeline {
 agent any
  
 tools {
    maven 'Maven'
 }
  
 stages {
   stage ('Initialise'){
     steps {
       sh '''
                    echo "PATH = ${PATH}"
                    echo "M2_HOME = ${M2_HOME}"
            '''
     }
   }
  
  
   
   stage ('SAST') {
      steps {
        withSonarQubeEnv('sonar') {
          sh 'mvn sonar:sonar'
          sh 'cat target/sonar/report-task.txt'
        }
      }
    }
  
  stage ('Build'){
     steps {
      sh 'mvn clean package' 
     }
   }
  
  
  
   stage ('Deploy-To-Tomcat') {
            steps {
           sshagent(['tomcat']) {
                sh 'scp -o StrictHostKeyChecking=no target/*.war ubuntu@18.224.56.101:/prod/apache-tomcat-9.0.44/webapps/webapp.war'
              }      
           }       
    }
  
 }
}
