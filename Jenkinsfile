node('master') {
    stage ( 'git checkout' ) {
       git 'https://github.com/shekharshamra/jenkin.git' 
    }
    
     stage ('Maven') {
     def dockerImage = 'maven:slim'
      docker.image(dockerImage).inside("-v ${WORKSPACE}:/root ") {
      sh " 'mvn' -Dmaven.test.failure.ignore clean install "
      }
  } 
     stage ('Junit'){
     junit '**/target/surefire-reports/TEST-*.xml'
     archive 'target/*.jar'
   }
    stage ('ArchiveArtifacts'){
	 echo 'Archiving Artifacts For the Build'
     archiveArtifacts artifacts: 'in28minutes-web-servlet-jsp/target/in28minutes.war', onlyIfSuccessful: true
   }
   stage ('NexusArtifactUploader'){
		 echo 'Upload artifacts in Nexus'
		 nexusArtifactUploader artifacts: [[artifactId: 'project', classifier: '', file: 'in28minutes-web-servlet-jsp/target/in28minutes.war', type: 'in28minutes.war']], credentialsId: 'Nexus', groupId: 'com.in28minutes', nexusUrl: '10.207.16.229:8082/nexus', nexusVersion: 'nexus2', protocol: 'http', repository: 'demodocker', version: '1.0-SNAPSHOT'
   }
}