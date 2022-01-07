pipeline {
  agent any
  triggers {
    githubPush()
    pollSCM('H H * * *')
  }
  environment {
    MAIL_RECIPIENTS = 'dev+tests-reports@wazo.io'
  }
  stages {
    stage ('Prepare') {
      steps {
        script {
          version = sh(script: 'dpkg-parsechangelog --show-field version', returnStdout: true).trim()
          currentBuild.displayName = "${JOB_NAME} ${version}"
          currentBuild.description = "Build Debian package ${JOB_NAME} ${version}"
        }
      }
    }
    stage ('Build package') {
      steps {
        build job: 'build-package', parameters: [
          string(name: 'PACKAGE', value: "${JOB_NAME}"),
          string(name: 'FORCED_VERSION', value: "${version}"),
          string(name: 'DEBIAN_REPOSITORY', value: 'private'),
          string(name: 'DEBIAN_DISTRIBUTION', value: 'nestbox-dev-buster'),
        ]
      }
    }
  }
  post {
    failure {
      emailext to: "${MAIL_RECIPIENTS}", subject: '${DEFAULT_SUBJECT}', body: '${DEFAULT_CONTENT}'
    }
    fixed {
      emailext to: "${MAIL_RECIPIENTS}", subject: '${DEFAULT_SUBJECT}', body: '${DEFAULT_CONTENT}'
    }
  }
}
