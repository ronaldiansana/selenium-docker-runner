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
                bat "docker-compose -f test-suites.yaml up"
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