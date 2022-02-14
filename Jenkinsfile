pipeline {

  agent any
  stage("Create dockerimage"){
    steps{
      sh "docker build -t vinayadav99/newapp:${BUILD_ID} ."
    }
  }
  stage("push image to repo"){
    steps{
      withCredentials([string(credentialsId: 'mypass', variable: 'pass')]) {
        sh "docker login -u vinayadav99 -p ${pass}"
        sh docker push vinayadav99/newapp:${BUILD_ID}"
      }
    }
  }
  stage("K8s deployment"){
    steps{
      sh "chmod +x myscript.sh"
      sh "sh myscript.sh ${BUILD_ID}"
      sshagent(['kops']) {
        sh "scp -o StrictHostKeyChecking=no newdep.yaml:/home/ubuntu"
        sh "ubuntu@13.235.100.183 kubectl create -f newdep.yaml
      } 
      
    }
  }
}
