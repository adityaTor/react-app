node {
  try {
    stage('Checkout') {
      checkout scm
    }
    stage('Environment') {
      sh 'git --version'
      echo "Branch: ${env.BRANCH_NAME}"
      sh 'docker -v'
      sh 'printenv'
    }
    stage('Build Docker test'){
      def testImage = docker.build("react-test", "-f Dockerfile.test .")
    }
    stage('Docker test'){
      testImage.withRun('--rm'){}
    }
    stage('Clean Docker test'){
      sh 'docker rmi react-test'
    }
    stage('Deploy'){
      if(env.BRANCH_NAME == 'master'){
        docker.withRegistry('https://index.docker.io/v1/', '218de882-021c-45d2-83e0-5c3ee8fdf6e6') {
          def app = docker.build("isthisreallife/node_js-example", '.').push()
        }
      }
    }
  }
  catch (err) {
    throw err
  }
}
