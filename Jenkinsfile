pipeline {
  agent any
  
    environment {
           def uploadSpec = """{
           "files": [
               {
            "pattern": "**/target/*.jar",
            "target": "libs-snapshot-local"
          }, {
            "pattern": "**/target/*.pom",
            "target": "libs-snapshot-local"
          }, {
            "pattern": "**/target/*.war",
            "target": "libs-snapshot-local"
          }
           ]
    }"""
        
        
    }
  
  stages {
    stage('Build & Unit Test') {
      steps {
        bat 'mvn clean package -DskipTests=true'
        
         
      }
	  
    }

 
  stage ('upload') {
    gitlabCommitStatus("upload") {
      def server = Artifactory.server "artifactory@ibsrv02"
      def buildInfo = Artifactory.newBuildInfo()
      buildInfo.env.capture = true
      buildInfo.env.collect()

      def uploadSpec = """{
        "files": [
          {
            "pattern": "**/target/*.jar",
            "target": "libs-snapshot-local"
          }, {
            "pattern": "**/target/*.pom",
            "target": "libs-snapshot-local"
          }, {
            "pattern": "**/target/*.war",
            "target": "libs-snapshot-local"
          }
        ]
      }"""
      // Upload to Artifactory.
      server.upload spec: uploadSpec, buildInfo: buildInfo

      buildInfo.retention maxBuilds: 10, maxDays: 7, deleteBuildArtifacts: true
      // Publish build info.
      server.publishBuildInfo buildInfo
 }}
/*    stage('Deploy CloudHub') {
      environment {
        ANYPOINT_CREDENTIALS = credentials('anypoint.credentials')
      }
      steps {
        bat 'mvn deploy -P cloudhub -Dmule.version=4.1.4 -Danypoint.username=RanjeetKushwahaCap -Danypoint.password=Raj@2018'
      }
    }
	
	*/
  }
}