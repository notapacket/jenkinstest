pipeline {
  agent any
  stages {
    stage('build') {
      steps {
        println env.BRANCH_NAME
        println env.dump()
        println gitCommitShort()
        println gitCommitCount()
        println getVersion()
      }
    }
    stage('run') {
      steps {
        sh 'exit 0'
      }
    }
  }
}

def gitCommitShort() {
    return sh(returnStdout: true, script: 'git rev-parse HEAD').trim().take(8)
}

def gitCommitCount() {
    return sh(returnStdout: true, script: 'git rev-list --count HEAD').trim()
}

def getVersion(versionFile = false) {
  def versionString = ""
  if (versionFile) {
    def versionFileContent = ""
    if (new File('src/main/resources/VERSION.txt').exists()) {
      def versionCommand = 'grep -E \'^version=.*$\' src/main/resources/VERSION.txt | head -n1 | cut -d"=" -f2'
      versionFileContent = sh(returnStdout: true, script: versionCommand)
    } else if (new File('VERSION.txt').exists()) {
      def versionCommand = 'grep -E \'^version=.*$\' VERSION.txt | head -n1 | cut -d"=" -f2'
      versionFileContent = sh(returnStdout: true, script: versionCommand)
    }
    versionString += versionFileContent
  }
  if (env.BRANCH_NAME != "master") {
    versionString += '-' + env.BRANCH_NAME.replaceAll('/','-')
  }
  versionString += '-' + gitCommitCount() + '-' + gitCommitShort() + '-' + env.BUILD_NUMBER
  if (!versionFile) {
    sh "echo 'version=" + versionString + "' > VERSION.txt"
  }
  return versionString
}
