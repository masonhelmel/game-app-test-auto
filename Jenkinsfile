pipeline
{
    agent any
    environment {
        // Docker Hub credentials ID stored in Jenkins
        DOCKER_HUB_CREDENTIALS = 'Cyber-3120'
        IMAGE_NAME = 'helmasw/appgametestauto'
    }

    stages
    {
        stage('Cloning Git')
        {
            steps
            {
                checkout scm
            }
        }
    }


}