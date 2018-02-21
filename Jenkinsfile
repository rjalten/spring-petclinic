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
   stage('Deploy to Node1') {
      sshagent(['sshkey_node1']) {
         sh 'scp -o StrictHostKeyChecking=no target/*.jar vagrant@192.168.100.11:'
         sh 'ssh vagrant@192.168.100.11 sudo systemctl restart petclinic'
         sleep 15
      }
   }
   stage('Smoketest Curl Node1:8080'){
      sh 'curl 192.168.100.11:8080 > /dev/null'
   }
   if (env.BRANCH_NAME == 'master'){
      stage('To Node2 if branche is Master'){
         sshagent(['sshkey_node2']) {
            sh 'scp -o StrictHostKeyChecking=no target/*.jar vagrant@192.168.100.12:'
            sh 'ssh vagrant@192.168.100.12 sudo systemctl restart petclinic'
         }
      }
      stage('Test working on Node2'){
         sh 'curl 192.168.100.12:8080 > /dev/null'
      }
   }
 
}
