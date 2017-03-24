pipeline {
  agent { docker { image 'ruby:2.4' } }
  stages {
    stage('build') {
      steps {
        sh './bin/bundle install'
        sh './bin/rails db:migrate'
        sh './bin/rails test'
      }
    }
  }
}
