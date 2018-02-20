node {
   def mvnHome
   stage('Preparation') { // for display purposes
      // Get some code from a GitHub repository
      git 'https://github.com/rjalten/spring-petclinic'
      // Get the Maven tool.
      // ** NOTE: This 'M3' Maven tool must be configured
      // **       in the global configuration.           
      //ook weg//mvnHome = tool 'M3'
   }
   stage('Build') {
      // Run the maven build
      if (isUnix()) {
         sh "'./mvnw' -Dmaven.test.failure.ignore clean package"
         //sh "'${mvnHome}/bin/mvn' -Dmaven.test.failure.ignore clean package"
      } else {
         bat(/"${mvnHome}\bin\mvn" -Dmaven.test.failure.ignore clean package/)
      }
   }
   stage('Results') {
      junit '**/target/surefire-reports/TEST-*.xml'
      archive 'target/*.jar'
   }
   stage('DeployToNode1') {
      sshagent(['sshkey_node1']) {
         sh 'scp -o StrictHostKeyChecking=no -l vagrant target/*.jar 192.168.100.11:'
         sh 'ssh vagrant@192.168.100.11 sudo systemctl restart petclinic'
      }
   }

}
