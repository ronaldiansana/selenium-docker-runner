pipeline{

    agent any

    stages{

        stage('Run Tests'){
            steps{
                bat "docker-compose up"
            }
        }

        stage('Bring Grid Down'){
            steps{
                bat "docker-compose down"
            }
        }
    }

}