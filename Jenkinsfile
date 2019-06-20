pipeline {
  agent any
  stages {
    stage('checkout') {
      steps {
        checkout scm
      }
    }
    stage('Install Modules') {
     steps {
            echo 'Installing node modules...'
            sh 'npm install'
          }
      }
    stage('Test') {
    steps {
              echo 'Testing...'
              sh 'npm run test --silent'
            }
            post{
             failure {
                emailext body: '${PROJECT_NAME} - Build # ${BUILD_NUMBER} - ${BUILD_STATUS}:Check console output at ${BUILD_URL} to view the results.',
                recipientProviders: [developers(), requestor()],
               subject: '${PROJECT_NAME} - Build # ${BUILD_NUMBER} - ${BUILD_STATUS}!',
               to: 'mrsvickyvampire@gmail.com'
                   }
              always{
                fileOperations([
                folderCreateOperation('${JENKINS_HOME}/userContent/sample-pruebita'),
                folderCreateOperation('${JENKINS_HOME}/userContent/sample-pruebita/${JOB_BASE_NAME}'),
                fileCopyOperation(excludes: '',
                flattenFiles: false,
                includes: 'src/tests/*.html',
                targetLocation: '${JENKINS_HOME}/userContent/sample-pruebita//${JOB_BASE_NAME}'),
                fileRenameOperation(source: '${JENKINS_HOME}/userContent/sample-pruebita//${JOB_BASE_NAME}/src/tests/units.html',destination: '${JENKINS_HOME}/userContent/sample-pruebita/${JOB_BASE_NAME}/src/tests/results_${BUILD_NUMBER}.html')])
            }
            }
      }
    stage('Build'){
      steps{
         sh ' npm run build -- --prod'
       }
        post {
           always {
                archiveArtifacts(artifacts: 'dist/', onlyIfSuccessful: true)
               }
      }
      }
    }
    }

