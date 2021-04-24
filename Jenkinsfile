CODE_CHANGES = getGitChanges()

def gv
pipeline {
  
  agent any
  environment {
    NEW_VERSION = '1.3.0'
  }

  parameters {
      // string(name: 'VERSION', defaultValue: '', description: 'version to deploy on prod')
      choice(name: 'VERSION', choices: ['1.1.0', '1.2.0', '1.3.0'], description: '')
      booleanParam(name: 'executeTests', defaultValue: true, description: '')
  }
  
  // gradle, maven, jdk only , make maven or gradle command availdable on all stages
  tools {
      maven 'Maven'
      gradle 'Gradle-6.2'
  }
    
  stages {
     
    stage("run frontend") {
      when {
        expression {
          // BRANCH_NAME == 'dev' && CODE_CHANGES == true
          params.executeTests == true // or params.executeTests
        }
      }
      steps {
          echo 'executing yarn...'
          echo "building version ${NEW_VERSION}"
          nodejs('Node-10.17') {
              sh 'yarn install'
          }
      }
    }
    
    
    stage("run backend") {
      when {
        expression {
          BRANCH_NAME == 'dev' || BRANCH_NAME == 'master'
        }
      }
      steps {
           echo 'executing gradle...'
           withCredentials([
             usernamePassword(credentials: 'server-credentials', usernameVariable: USER, passowrdVariable: PWD)
           ]){
             sh "some script ${USER} ${PWD}"
           }
           sh './gradlew -v'    
      }
    }

    stage("init") {
      steps {
        script {
          // groovy script
          gv = load "script.groovy"
        }
      }
    } 

    stage("build") {
      steps {
        script {
          // groovy script
          gv.buildApp()
        }
      }
    } 
  }

  post {
    always {
      // 
    }

    success {
      //
    }

    failure {
      //
    }
  }
}
