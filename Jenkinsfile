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
        
    /*stage('Publish') {
     nexusPublisher nexusInstanceId: 'TCS_Hackathon_15', nexusRepositoryId: 'releases', packages: [[$class: 'MavenPackage', mavenAssetList: [[classifier: '', extension: '', filePath: '/var/lib/jenkins/workspace/TCS_Hackathon_15/eureka-server/target/eureka-server-0.0.1-SNAPSHOT.jar']], mavenCoordinate: [artifactId: 'spring-cloud-eureka-example', groupId: 'org.exampledriven', packaging: 'jar', version: '0.0.1-SNAPSHOT']]]
   }*/
        stage('Publish') {
                
                script {
                    // Read POM xml file using 'readMavenPom' step , this step 'readMavenPom' is included in: https://plugins.jenkins.io/pipeline-utility-steps
                    pom = readMavenPom file: "pom.xml";
                    // Find built artifact under target folder
                    filesByGlob = findFiles(glob: "target/*.jar");
                    // Print some info from the artifact found
                    echo " print ${filesByGlob[0].name} ${filesByGlob[0].path} ${filesByGlob[0].directory} ${filesByGlob[0].length} ${filesByGlob[0].lastModified}"
                    // Extract the path from the File found
                    artifactPath = filesByGlob[0].path;
                    // Assign to a boolean response verifying If the artifact name exists
                    artifactExists = fileExists artifactPath;
                        echo "artifactExists ${artifactExists}";
                    if(artifactExists) {
                        echo "*** File: ${artifactPath}, group: ${pom.groupId}, packaging: ${pom.packaging}, version ${pom.version}";
                        nexusArtifactUploader(
                            nexusVersion: nexus3,
                            protocol: http,
                            nexusUrl: '104.197.71.163:8081',
                            groupId: pom.groupId,
                            version: '${BUILD_NUMBER}',
                            repository: release,
                            credentialsId: '003a7a3f-9533-3504-bc59-860f1d154ba3',
                            artifacts: [
                                // Artifact generated such as .jar, .ear and .war files.
                                [artifactId: pom.artifactId,
                                classifier: '',
                                file: artifactPath,
                                type: jar],
                                // Lets upload the pom.xml file for additional information for Transitive dependencies
                                [artifactId: pom.artifactId,
                                classifier: '',
                                file: "pom.xml",
                                type: "pom"]
                            ]
                        );
                    } else {
                        error "*** File: ${artifactPath}, could not be found";
                    }
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
