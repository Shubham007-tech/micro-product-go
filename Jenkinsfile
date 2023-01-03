pipeline {
    
    agent {
     label 'go16'
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
          
           stage("unit-test") {
            steps {
                container ('go') {
                   echo 'UNIT TEST EXECUTION STARTED'
                   sh 'make unit-tests'
                }
               
            }
        }
        stage("functional-test") {
            steps {
                container ('go') {
                    echo 'FUNCTIONAL TEST EXECUTION STARTED'
                    sh 'make functional-tests'
       
             }
  
                
            }
        }
       
           
          
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
          
          
         	    
	    stage('build & push image on harbor') {
          steps {
            container('go') {
                  sh 'docker build -t $REGISTRY/$HARBOR_NAMESPACE/$APP_NAME:2.0.0-SNAPSHOT .'
                  withCredentials([usernamePassword(passwordVariable: 'HARBOR_PASSWORD', usernameVariable: 'HARBOR_USERNAME', credentialsId: "$HARBOR_CREDENTIAL_ID",)]) {
                                sh 'echo "$HARBOR_PASSWORD" | docker login $REGISTRY -u "$HARBOR_USERNAME" --password-stdin'
                                sh 'docker push $REGISTRY/$HARBOR_NAMESPACE/$APP_NAME:2.0.0-SNAPSHOT'
        }
      }
    }
  }
	    stage("Remove the local Docker image") {
      steps {
        container('go') {
          sh '''
            docker image rm $REGISTRY/$HARBOR_NAMESPACE/$APP_NAME:2.0.0-SNAPSHOT
          '''
        }
      }
    }


      
      
    }
}
