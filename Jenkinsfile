pipeline {
    
    agent {
     label 'go'
   }
    
  environment {
    HARBOR_CREDENTIAL_ID = 'harbor-id'
    REGISTRY = '172.30.102.24:30002'
    HARBOR_NAMESPACE = 'library'
    APP_NAME = 'demogolang-two'
   
    GO114MODULE = 'on'
    CGO_ENABLED = 0 
    GOPATH = "${JENKINS_HOME}/jobs/${JOB_NAME}/builds/${BUILD_ID}"
    
  }
  
   
  
  
  
      stages {
       
          
          
          
        stage("build") {
            steps {
                container ('go') {
                   echo 'BUILD EXECUTION STARTED'
                   sh 'go version'
                   sh 'go get ./...'
                   sh 'docker build . -t shadowshotx/product-go-micro'
       
                }
               
            }
        }
      
      
    }
}
