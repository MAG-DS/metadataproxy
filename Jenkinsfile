node {
    def base
    def dockerhub  = 'https://dockerhub.mago-data.com'
    def commitHash
    def tag
    def branch

    stage('Clone repository') {
        def scmVars = checkout scm
        commitHash = scmVars.GIT_COMMIT
        branch = scmVars.GIT_BRANCH
        tag = sh(returnStdout: true, script: "git tag --contains | head -1").trim()
    }

    stage('Build base image') {
        /* This builds the actual image; synonymous to
         * docker build on the command line */

        base = docker.build("iam-metadataproxy")
    }

	stage('push base image') {
      docker.withRegistry(dockerhub) {
        if (branch == 'master') {
            base.push("master-${BUILD_NUMBER}")
            base.push("latest")
        }
        if (tag) {
          echo "pushing base:${tag}"
          base.push(tag)
        }
      }
    }

}
