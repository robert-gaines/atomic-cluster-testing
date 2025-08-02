pipeline {
                agent {
                    label 'res-phy-prd-rpi-9'  
                }

    environment {
        KUBECONFIG = "/etc/rancher/k3s/k3s.yaml"
        NAMESPACE = "default"
        WORKSPACE = "/code/atomic-cluster-testing"
        GIT_CREDENTIALS_ID = '88a9dcb0-74dc-44cd-91cd-2ff54e337e1e'  
        GIT_REPOSITORY_URL = 'https://github.com/robert-gaines/atomic-cluster-testing.git'
        GIT_BRANCH = 'main'
        WEBHOOK_URL = 'https://chat.internal.subterfuge.biz/hooks/ixsizrcwa7y35gtwn6mw3mr37a'
    }
    stages {
        stage('Checkout') {
            steps {
                git(
                    credentialsId: "${env.GIT_CREDENTIALS_ID}",
                    url: "${env.GIT_REPOSITORY_URL}",
                    branch: "${env.GIT_BRANCH}"
                )
            }
        }

        stage('Apply Job Manifest') {
            steps {
                
                script {
            
                    sh "kubectl apply -f ${env.WORKSPACE}/AtomicRed-Job.yaml --namespace=${env.NAMESPACE}"

                }
            }
        }

        stage('Verify Job Creation') {
            steps {
                script {
                    // Check the status of the deployed pod
                    sh "kubectl get jobs --namespace=${env.NAMESPACE}"
                }
            }
        }
    }

    post {
        success {
            script {
                def message = "Atomic Red Team Cluster Test Pipeline Execution Succeeded!"
                sh """
                curl -k -X POST -H 'Content-Type: application/json' -d '{"text": "${message}"}' ${env.WEBHOOK_URL}
                """
            }
        }
        failure {
            script {
                def message = "Atomic Red Team Cluster Test Pipeline Execution Failed."
                sh """
                curl -k -X POST -H 'Content-Type: application/json' -d '{"text": "${message}"}' ${env.WEBHOOK_URL}
                """
            }
        }
        always {
            script {
                // Add any cleanup or notification steps here
                cleanWs()
                echo 'Pipeline completed.'
            }
        }
    }
}
