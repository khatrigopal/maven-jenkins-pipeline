pipeline {
  agent any
  tools {
        maven "maven"
   }

  stages {
      stage('Build Artifact') {
            steps {
              sh "mvn clean package -DskipTests=true"
              archive 'target/*.jar' 
            }  
       }
      stage('Test Maven - JUnit') {
            steps {
              sh "mvn test"
            }
            post{
              always{
                junit 'target/surefire-reports/*.xml'
              }
            }
        }

    stage('Sonarqube Analysis - SAST') {
            steps {
                  
           sh "mvn clean verify sonar:sonar \
  -Dsonar.projectKey=maven-jenkins-pipeline \
  -Dsonar.projectName='maven-jenkins-pipeline' \
  -Dsonar.host.url=http://54.163.212.58:9000 \
  -Dsonar.token=sqp_50db48f65b678ae8314eef3ff4b5b54ec256ba6d \
            
            }
           timeout(time: 2, unit: 'MINUTES') {
                      script {
                        waitForQualityGate abortPipeline: true
                    }
                
              }
        }
        

      
     }
}
