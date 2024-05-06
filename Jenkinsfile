pipeline {
    agent any

    triggers {
        pollSCM('H/1 * * * *')
    }

    tools {
        maven "Maven"
    }

    stages {
        stage('Build') {
            steps {
                checkout scm
                script {
                    sh "mvn clean package assembly:single"
                }
            }
        }

        stage('Run JAR') {
            steps {
                script {
                    sh "java -jar target/home-task-1.0-jar-with-dependencies.jar > output.log"
                }
            }
        }
    }

    post {
        success {
            archiveArtifacts artifacts: 'target/home-task-1.0-jar-with-dependencies.jar'
            archiveArtifacts artifacts: 'output.log'

            script {
                def artifactName = "hello-${env.BUILD_NUMBER}.tar.gz"
                sh "tar -czf ${artifactName} target/home-task-1.0.jar output.log"
                archiveArtifacts artifacts: "${artifactName}", fingerprint: true
            }
        }
    }
}