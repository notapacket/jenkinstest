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

def getVersionFileVersion() {
    def versionFile = ""
    if (fileExists('src/main/resources/VERSION.txt')) {
        versionFile = 'src/main/resources/VERSION.txt'
    } else if (fileExists('VERSION.txt')) {
        versionFile = 'VERSION.txt'
    } else {
        return ""
    }
    return sh(returnStdout: true, script: "grep -E '^version=.*\$' ${versionFile} | head -n1 | cut -d'=' -f2").trim()
}

def getVersion() {
  def versionString = getVersionFileVersion()
  def versionSuffix = "${gitCommitCount()}-${gitCommitShort()}-${env.BUILD_NUMBER}"
  def createVersionFile = (versionString == "") ? true : false
  if (env.BRANCH_NAME != "master") {
      def branchName = env.BRANCH_NAME.replaceAll('/', '-')
      versionString += (versionString == "") ? branchName : "-${branchName}"
  }
  versionString += (versionString == "") ? versionSuffix : "-${versionSuffix}"
  if (createVersionFile) {
      sh "echo 'version=${versionString}' > VERSION.txt"
  }
  return versionString
}
