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

        stage('SCA-SAST-TEST')
        {
            agent any
            steps
            {
                script
                {
                    snykSecurity(
                        snykInstallation: 'Snyk-Install',
                        snykTokenId: 'Snyk-API-token',
                        severity: 'critical'
                    )
                }
            }
        }

        stage('Build-and-Tag')
        {
            agent { label '3120-lab1-Appserver'}

            steps
            {
                script
                {
                    // Build Docker image using Jenkins Docker pipeline plugin
                    echo "Building Docker image ${IMAGE_NAME}..."
                    app = docker.build("${IMAGE_NAME}")
                    app.tag("latest")

                }
            }
        }

        stage('Push-to-DockerHub')
        {
            agent { label '3120-lab1-Appserver'}

            steps
            {
                script
                {
                    echo "Pushing Docker image ${IMAGE_NAME}:latest to Docker Hub..."
                    docker.withRegistry('https://registry.hub.docker.com', "${DOCKER_HUB_CREDENTIALS}")
                    {
                        app.push("latest")
                    }
                }
            }
        }

        stage('DAST')
        {
            steps
            {
                sh 'echo Running DAST scan with snyk...'
            }
        }

        stage('DEPLOYMENT')
        {
            agent { label '3120-lab1-Appserver'}

            steps
            {
                echo 'Starting Deployment using docker-compose...'
                script
                {
                    dir("${WORKSPACE}")
                    {
                        sh '''
                            docker-compose down
                            docker-compose up -d
                            docker ps
                        '''
                    }
                    
                }
                echo 'Deployment Completed'
            }
            
        }
        

    }


}
