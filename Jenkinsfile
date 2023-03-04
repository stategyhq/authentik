node {
    def app
    environment {
                  registryCredential = 'dockerhub'
                  registry="https://hub.docker.com/r/stategyhq/authentik"
              }
    parameters {
    string(name: 'MY_PARAM', defaultValue: 'default_value', description: 'My parameter')
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
    stage('Remove Unused docker image') {
  
       sh "docker rmi stategyhq/authentik:$BUILD_NUMBER"
  
    }

    stage ('Trigger Update Manifest') {
           
            echo "Triggering Update Manifest Job"
            def buildNumberString = env.BUILD_NUMBER.toString()
            echo buildNumberString
            def DOCKERTAG = [string(name: 'MY_PARAM', value: buildNumberString)]
            build job: 'authentik-manifest', parameters: DOCKERTAG
                
        }
}
