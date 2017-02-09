node {
   stage('Preparation') { // for display purposes
      // Get some code from a GitHub repository
      git 'https://github.com/partsunlimitedmrp/orderAPI.git'
   }
   stage('Build') {
      sh 'docker build -t partsunlimitedmrp/orderapi:${BUILD_ID} .'
   }
   stage('Push') 
   withCredentials([[$class: 'UsernamePasswordMultiBinding', credentialsId: 'dockerhub',
                    usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD']])
   {
      sh 'docker login --username=$USERNAME --password=$PASSWORD' 
      sh 'docker push partsunlimitedmrp/orderapi:${BUILD_ID}'
   }
   stage('Prepare Breeds') 
   {
      sh 'sed -i \'s/IDTAGA/\'${BUILD_ID}\'/g\' deploy/pumrporder50.yaml'
      sh 'sed -i \'s/IDTAGB/\'$((${BUILD_ID}-1))\'/g\' deploy/pumrporder50.yaml'
   } 
   stage('Deploy 50') 
   {
       sh 'curl -v -X POST --data-binary @deploy/pumrporder50.yaml -H "Content-Type: application/x-yaml" vamp.vamp.marathon.mesos:12061/api/v1/deployments'
   }
}
