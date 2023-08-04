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
                  withSonarQubeEnv('SonarQube') {
           sh "mvn sonar:sonar \
                              -Dsonar.projectKey=test-project \
                        -Dsonar.host.url=http://3.85.2.147:9000" 
                }
           timeout(time: 2, unit: 'MINUTES') {
                      script {
                        waitForQualityGate abortPipeline: true
                    }
                }
              }
        }
        }
     }
