node {
    def base
    def dockerhub  = 'https://dockerhub.mago-data.com'
    def commitHash
    def tag

    stage('Clone repository') {
        def scmVars = checkout scm
        commitHash = scmVars.GIT_COMMIT
        tag = sh(returnStdout: true, script: "git tag --contains | head -1").trim()
    }

    stage('Build base image') {
        /* This builds the actual image; synonymous to
         * docker build on the command line */

        base = docker.build("iam-metadataproxy")
    }

	stage('push base image') {
      docker.withRegistry(dockerhub) {
        base.push(commitHash)
        base.push("latest")
        if (tag) {
          echo "pushing base:${tag}"
          base.push(tag)
        }
      }
    }

}
