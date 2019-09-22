node {
        def app
        stage('Clone repository') {
        /* Let's make sure we have the repository cloned to our workspace */
        checkout scm
     }
        stage('Build Project') {
                sh 'mvn clean package'
        }
       
       stage('SonarQube analysis') {
       def mvnHome = tool name: 'localmaven' , type: 'maven'
       withSonarQubeEnv('sonarqube') {
       sh "${mvnHome}/bin/mvn sonar:sonar"
       //sh 'mvn org.sonarsource.scanner.maven:sonar-maven-plugin:3.2:sonar'
    }
  }

       stage('Build image') {
        /* This builds the actual image; synonymous to
         * docker build on the command line */
       
       sh 'docker build -t trydocker29/eureka-service:prod ./eureka-server'
      }
        
    stage('Publish') {
     nexusPublisher nexusInstanceId: 'TCS_Hackathon_15', nexusRepositoryId: 'releases', packages: [[$class: 'MavenPackage', mavenAssetList: [[classifier: '', extension: '', filePath: '/var/lib/jenkins/workspace/TCS_Hackathon_15/eureka-server/target/eureka-server-0.0.1-SNAPSHOT.jar']], mavenCoordinate: [artifactId: 'eureka-server', groupId: 'org.jenkins-ci.main', packaging: 'jar', version: '2.23']]]
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
