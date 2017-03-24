pipeline {
  agent { docker { image 'ruby:2.4' } }
  stages {
    stage('build') {
      steps {
        sh '''
          apt-get update && apt-get install -y nodejs
          bundle install
          ./bin/rails db:migrate
          ./bin/rails test
        '''
      }
    }
    stage('run') {
      steps {
        sh '''
          sleep 10
        '''
      }
    }
  }
}
