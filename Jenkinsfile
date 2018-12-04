pipeline {
  agent any
  
    environment {
           def uploadSpec = """{
           "files": [
               {
               "pattern": "target/*.zip",
                   "target": "libs-snapshot-local/mulesoft_integration/"
               }
           ]
    }"""
        
        
    }
  
  stages {
    stage('Build & Unit Test') {
      steps {
        bat 'mvn clean package -DskipTests=true'
        
          script {
                                        def server = Artifactory.newServer url: 'http://localhost:8081/artifactory', credentialsId: 'mulesoft-artifactory'
                                        server.bypassProxy = true
                                        def buildInfo = server.upload spec: uploadSpec
                                        } 
      }
    }

 
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