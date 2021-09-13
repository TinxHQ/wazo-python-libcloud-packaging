pipeline {

  agent any

  triggers {
    pollSCM('H H * * *')
  }

  parameters {
    string(name: 'DEBIAN_REPOSITORY', defaultValue: 'private', description: 'Debian repository name')
    string(name: 'DEBIAN_DISTRIBUTION', defaultValue: 'nestbox-dev-buster', description: 'Debian distribution name')
  }

  stages {
    stage ('Prepare') {
      steps {
        script {
          // Cleanup
          deleteDir()

          // Get sources, replace checkout scm
          git(credentialsId: 'github-jenkins-wazo-bot', url: "https://github.com/TinxHQ/${JOB_NAME}")

          // Get package version as var from sources
          version = sh (
                script: 'wazo-version unstable',
                returnStdout: true
            ).trim()

          // Rename build
          currentBuild.displayName = "${JOB_NAME} ${version}"
          currentBuild.description = "Build Debian package ${JOB_NAME} ${version}"
        }
      }
    }

    stage ('Build package') {
      steps {
        build job: 'build-debian-package', parameters: [
          string(name: 'PACKAGE', value: "${JOB_NAME}"),
          string(name: 'VERSION', value: "${version}"),
          string(name: 'DEBIAN_REPOSITORY', value: "${params.DEBIAN_REPOSITORY}".trim()),
          string(name: 'DEBIAN_DISTRIBUTION', value: "${params.DEBIAN_DISTRIBUTION}".trim())
        ]
      }
    }

  }

}
