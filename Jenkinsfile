pipeline {
    agent any
        tools {
        maven 'mvn'
        jdk 'JDK'
    }
    stages {
        stage ('Initialize') {
            steps {
                sh '''
                    echo "PATH = ${PATH}"
                    echo "M2_HOME = ${M2_HOME}"
                '''
            }
        }
         stage ('Build') {
            steps {
                sh '''cd TestMaven
                mvn -Dmaven.test.failure.ignore=true install
                '''
            }
            post {
                success {
                    sh 'echo $pwd'
                    junit 'TestMaven/target/surefire-reports/TEST-udemy.AppTest.xml' 
                }
            }
        }
        stage ('Sonar') {
            steps {
                script {
                            // Github plugin for PR's (does not submit to server)
                            sh '~/sonar-scanner-3.0.3.778-linux/bin/sonar-scanner'
                          
                }
    }
        }
         stage ('Artifactory configuration') {
             
          // Obtain an Artifactory server instance, defined in Jenkins --> Manage:
            
             steps {
                          
                  script {
                      def server = Artifactory.server 'AF1'
                      def uploadSpec = """{
                        "files": [
                            {
                             "pattern": "$WORKSPACE/TestMaven/target/*.jar",
                             "target": "libs-snapshot-local",
                             "flat": "false",
                             "recursive": "false"
                                                         }
                        ]
                    }"""
                      server.upload(uploadSpec)
             }
             }
         }
    }
}
