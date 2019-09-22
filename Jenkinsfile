pipeline {
    agent any
    stages {
        stage('Clone repository') {
        /* Let's make sure we have the repository cloned to our workspace */
        steps {
        checkout scm
     }
    }
        stage('Build Project') {
            steps {
                sh "mvn clean"
            }
        }
        stage('Build image') {
        /* This builds the actual image; synonymous to
         * docker build on the command line */
        steps {
        docker.build("trydocker29/eureka-service")
      }
    }
     /*   stage('--test--') {
            steps {
                sh "mvn test"
            }
        }
        stage('--package--') {
            steps {
                sh "mvn package"
            }
        }*/
    }
}
