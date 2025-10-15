pipeline {
    agent {
        label 'app'
    }

    triggers {
        pollSCM('* * * * *')
    }

    stages {
        stage('git clone') {
            steps {
                git url: 'https://github.com/pavan01234567/spring-petclinic.git',
                    branch: 'main'
            }
        }

        stage('build') {
            steps {
                sh 'mvn package'
            }
        }

        stage('sonarQube-scan') {
            steps {
                withCredentials([string(credentialsId: 'sonar_id', variable: 'SONAR_TOKEN')]) {
                    withSonarQubeEnv('sonarqube') {
                        sh """mvn package sonar:sonar \
                            -Dsonar.projectKey=pavan01234567_spring-petclinic \
                            -Dsonar.organization=pavan01234567 \
                            -Dsonar.host.url=https://sonarcloud.io \
                            -Dsonar.login=$SONAR_TOKEN"""
                    }
                }
            }
        }
    }

    post {
        always {
            junit '**/target/surefire-reports/*.xml'
            archiveArtifacts artifacts: '**/target/*.jar'
        }
        success {
            echo 'Build successful in sonar branch'
        }
        failure {
            echo 'Build failed'
        }
    }
}
