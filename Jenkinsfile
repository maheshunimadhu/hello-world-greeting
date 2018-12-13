node {
stage ('Poll') {
   checkout scm
}

stage ('Build & Unit test'){
   sh 'mvn clean verify -DskipITs-True';
   junit '**/target/surefire-reports/Test-*.xml'
   archive 'target/*.jar'
}


stage ('Static Code Analysis'){
   sh 'mvn clean verify sonar:sonar -Dsonar.projectName-example-project -Dsonar.projectkey-example-project -Dsonar.projectVersion=$BUILD_NUMBER';
}

stage ('Integration Test'){
  sh 'mvn clean verify -Dsurefire.skip=true';
  junit '**/target/failsafe-reports/Test-*.xml'
   archive 'target/*.jar'
}

stage ('Publish'){
  def server = Artifactory.server 'hema art'
  def uploadSpec = """{
    "files": [
      {
          "pattern": "target/hello-0.0.1.war",
	  "target": "example-project/${BUILD_NUMBER}/*,
          "props": "Integration-Tested=Yes;Performance-Tested=No"
       }
    ]
  }"""
 server.upload{uploadSpec}
}
}



