pipeline {

  agent any
  stages{

  
  stage("Create dockerimage"){
    steps{
      sh "docker build -t vinayadav99/newapp:${BUILD_ID} ."
    }
  }
  stage("push image to repo"){
    steps{
      withCredentials([string(credentialsId: 'mypass', variable: 'pass')]) {
        sh "docker login -u vinayadav99 -p ${pass}"
        sh "docker push vinayadav99/newapp:${BUILD_ID}"
      }
    }
  }
  stage("K8s deployment"){
    steps{
      sh "chmod +x myscript.sh"
      sh "sh myscript.sh ${BUILD_ID}"
      sshagent(['ubuntu']) {
        sh "scp -o StrictHostKeyChecking=no newdep.yaml ubuntu@52.66.201.78:/home/ubuntu"
        script{
          try{
            sh "ssh ubuntu@52.66.201.78 kubectl create -f newdep.yaml"
          }catch(error){
             sh "ssh ubuntu@52.66.201.78 kubectl apply -f newdep.yaml"
          }
        }

      } 
      
    }
  }
  }
}
