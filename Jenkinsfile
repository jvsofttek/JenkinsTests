pipeline {
  agent any
  
  stages {
    stage("build") {
      steps {
        echo 'building the application... '
      }
    }
    
    stage("test") {
      steps {
        echo 'testing the application2... '
      }
    }
    
    stage("deploy") {
      steps {
        echo 'deploying the application... '
      }
    }
  }
  post {
    always{
    }
    success{
    }
  }
}
