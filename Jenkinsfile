pipeline {
                agent {
                    label 'jenkins-runner'  
                }

    environment {
        KUBECONFIG = "/etc/rancher/k3s/k3s.yaml"
        NAMESPACE = "default"
        WORKSPACE = "/code/atomic-cluster-testing"
        GIT_CREDENTIALS_ID = ''  
        GIT_REPOSITORY_URL = 'https://github.com/robert-gaines/atomic-cluster-testing.git'
        GIT_BRANCH = 'main'
        WEBHOOK_URL = 'https://chat.server.fqdn/hooks/hook_id'
    }
    
    stages {
        
        stage('Delete existing job') {
            steps {
                script {
                         try {
                              sh "kubectl delete job atomicred --namespace=${env.NAMESPACE}"
                          } catch (Exception e) {
                              echo 'Exception raised: ' + e.toString()
                          }
                }
            }
        }
        
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
