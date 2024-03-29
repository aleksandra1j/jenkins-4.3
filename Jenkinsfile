node {
    def app

    stage('Clone repository') {
        checkout scm
    }

    stage('Build image') {
        if (env.BRANCH_NAME == 'dev') {
            app = docker.build("aleksandra1j/jenkins-4.3")
        } else {
            echo "Skipping Docker image build and push on branch ${env.BRANCH_NAME}"
        }
    }

    stage('Push image') {
        if (env.BRANCH_NAME == 'dev') {
            docker.withRegistry('https://index.docker.io/v1/', 'DockerHub') {
                app.push("${env.BRANCH_NAME}-${env.BUILD_NUMBER}")
                app.push("${env.BRANCH_NAME}-latest")
                // signal the orchestrator that there is a new version
            }
        } else {
            echo "Skipping Docker image push on branch ${env.BRANCH_NAME}"
        }
    }
}
