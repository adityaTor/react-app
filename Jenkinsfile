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
      sh 'docker build -t react-test -f Dockerfile.test --no-cache . '
    }
    stage('Docker test'){
      sh 'docker run --rm react-test'
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
