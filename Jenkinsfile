pipeline {
    
    agent any

    environment {
        RESULTS_DIR = 'results'
    }
    stages {
        stage('Source') {
            steps {
                git 'https://github.com/yax3126/unir-cicd.git'
            }
        }

        stage('Build') {
            steps {
                echo 'Building stage!'
                bat 'make build'
            }
        }

        stage('Unit tests') {
            steps {
                bat 'make test-unit'
                archiveArtifacts artifacts: "${RESULTS_DIR}/*.xml"
            }
        }

        stage('API tests') {
            steps {
                bat 'make test-api'
                archiveArtifacts artifacts: "${RESULTS_DIR}/api_*.xml"
            }
        }

        stage('E2E tests') {
            steps {
                bat 'make test-e2e'
                archiveArtifacts artifacts: "${RESULTS_DIR}/e2e_*.xml"
            }
        }
    }

    post {
        always {
            junit 'results/unit_result.xml'
            junit 'results/api_result.xml'
            junit 'results/e2e_result.xml' // ahora sí estará presente
            archiveArtifacts artifacts: 'results/**/*.*', allowEmptyArchive: true
            cleanWs()
        }
    

        failure {
            script {
                echo "El trabajo '${env.JOB_NAME}' falló en ejecución #${env.BUILD_NUMBER}"

                // Ejemplo construido por el generador de snippets de Jenkins:
                // mail bcc: '',
                //      body: "El trabajo ${env.JOB_NAME} ha fallado en la ejecución #${env.BUILD_NUMBER}.",
                //      cc: '',
                //      from: '',
                //      replyTo: '',
                //      subject: "Fallo en ${env.JOB_NAME} - Ejecución #${env.BUILD_NUMBER}",
                //      to: 'devops@fintechsolutions.com'
            }
        }
    }
}