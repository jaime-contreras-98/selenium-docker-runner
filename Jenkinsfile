pipeline {
    agent any
    parameters {
        choice choices: ['chrome', 'firefox'], description: 'Browser that will run tests', name: 'BROWSER'
    } 
    stages {
        stage('Start Grid in background') {
            steps {
                sh "docker-compose -f grid.yml up --scale ${params.BROWSER}=2 -d"
            }
        }
        stage('Run Test') {
            steps {
                sh "docker-compose -f test-suites.yml up"
            }
        }
    }

    post {
        always {
            sh "docker-compose -f grid.yml down"
            sh "docker-compose -f test-suites.yml down"
            archiveArtifacts artifacts: 'output/flight-reservation/emailable-report.html', followSymlinks: false
            archiveArtifacts artifacts: 'output/vendor-portal/emailable-report.html', followSymlinks: false
        }
    }
}