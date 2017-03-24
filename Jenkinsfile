pipeline {
  agent { docker { image 'nextrevision/ruby24-nodejs' } }
  stages {
    stage('build') {
      steps {
        sh '''
          bundle install
          ./bin/rails db:migrate
          ./bin/rails test
        '''
      }
    }
    stage('run') {
      steps {
        sh 'exit 0'
      }
    }
  }
}
