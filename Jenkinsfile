pipeline {
    agent any
    environment {
        APP_PORT = '9090'
        abracadabra = "${JOB_NAME}"
    }
    stages {
        stage ('Build') {
            steps {
                  echo "Starting Job ${abracadabra}..."
                  sh 'mvn -B package -Dskiptests'
            }
        }
        stage ('Integration Test') {
            parallel {
                stage ('Running Application') {
                    options {
                        timeout(time:60,unit:"SECONDS")
                    }
                    steps { 
                        script {
                            try {
                                dir('./target/'){
                                    sh 'java -jar contact.war'
                                }
                            } catch (Exception e) {
                                echo "SUCCESS!"
                            }
                        }
                    }
                }
                stage ('Running Test') {
                    steps {
                        sleep(time:30,unit:"SECONDS")
                        sh 'mvn -B -Dtest=RestIT test'    
                    }
                }
            }
        }
    }
}
