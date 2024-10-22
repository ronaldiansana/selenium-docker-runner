pipeline{

    agent any

    // generated from Jenkins > Job > Pipeline Syntax > Declarative Directive Generator
    // parameters and env variables are the same, but has difference on Jenkins UI Build Now button
    parameters {
        choice choices: ['chrome', 'firefox'], description: 'Select the browser', name: 'BROWSER'
    }

    stages{

        stage('Start Grid'){
            steps{
                bat "docker-compose -f grid.yaml up --scale ${params.BROWSER}=2 -d"
            }
        }

        stage('Run Tests'){
            steps{
                // --pull=always will always pull latest image from dockerhub
                bat "docker-compose -f test-suites.yaml up --pull=always"
                // Job should return FAIL if there's a failing test
                script {
                    if(fileExists('output/flight-reservation/testng-failed.xml') || fileExists('output/vendor-portal/testng-failed.xml')){
                        error('failed tests found')
                    }
                }
            }
        }
    }

    post {
        always {
            bat "docker-compose -f grid.yaml down"
            bat "docker-compose -f test-suites.yaml down"
            // generated from Jenkins > Job > Pipeline Syntax > Snippet Generator
            archiveArtifacts artifacts: 'output/flight-reservation/emailable-report.html', followSymlinks: false
            archiveArtifacts artifacts: 'output/vendor-portal/emailable-report.html', followSymlinks: false
        }
    }

}