node {
    def app
    environment {
                  registryCredential = 'dockerhub'
                  registry="https://hub.docker.com/r/stategyhq/authentik"
              }

    stage('Clone Repo') {
      checkout scm
    }

    stage('Environment') {
      sh 'git --version'
      echo "Branch: ${env.BRANCH_NAME}"
      sh 'docker -v'
      sh 'printenv'
    }


    stage('Build and push Docker Image'){

        app= docker.build("stategyhq/authentik:${env.BUILD_NUMBER}")
         docker.withRegistry('', 'dockerhub') {

        def customImage = docker.build("stategyhq/authentik:${env.BUILD_ID}")

        /* Push the container to the custom Registry */
        customImage.push()
    }
     echo "image built successfully"
    }
}
