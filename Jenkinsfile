pipeline {
    agent any
    environment {
        SERVICE_1_VERSION_NUMBER = sh(script: 'cat service1_version.txt ', returnStdout: true).trim()
        SERVICE_2_VERSION_NUMBER = sh(script: 'cat service2_version.txt ', returnStdout: true).trim()
        GITHUB_LINK = "https://github.com/mabutkovic/service2/raw/master"
    }
    stages {
        stage('Gather version numbers') {
            steps {
                script {
                    sh(script: "source /etc/profile; echo 'service1 version: '$SERVICE_1_VERSION_NUMBER")
                    sh(script: "source /etc/profile; echo 'service1 version: '$SERVICE_2_VERSION_NUMBER")
                }
            }
        }

        stage('Project1 - Staging - Service 1') {
            steps {
                script {
                    sh(script: "source /etc/profile; helm upgrade --set version=$SERVICE_1_VERSION_NUMBER p1-staging-service1-workflow $GITHUB_LINK/service1-workflow-0.1.0.tgz -f $GITHUB_LINK/service1-workflow/values.p1.staging.yaml")
                }
            }
        }
        stage('Project1 - Staging - Service 2') {
            steps {
                script {
                    sh(script: "source /etc/profile; helm upgrade --set version=$SERVICE_2_VERSION_NUMBER --set SERVICE1_URL=http://project1-staging-service1-svc:8080 p1-staging-service2-workflow $GITHUB_LINK/service2-workflow-0.1.0.tgz -f $GITHUB_LINK/service2-workflow/values.p1.staging.yaml")
                }
            }
        }
        stage('Project1 - Prod - Service 1') {
            steps {
                script {
                    sh(script: "source /etc/profile; helm upgrade --set version=$SERVICE_1_VERSION_NUMBER p1-prod-service1-workflow $GITHUB_LINK/service1-workflow-0.1.0.tgz -f $GITHUB_LINK/service1-workflow/values.p1.prod.yaml")
                }
            }
        }

        stage('Project1 - Prod - Service 2') {
            steps {
                script {
                    sh(script: "source /etc/profile; helm upgrade --set version=$SERVICE_2_VERSION_NUMBER --set SERVICE1_URL=http://project1-prod-service1-svc:8080 p1-prod-service2-workflow $GITHUB_LINK/service2-workflow-0.1.0.tgz -f $GITHUB_LINK/service2-workflow/values.p1.prod.yaml")
                }
            }
        }

        stage('Project2 - Staging - Service 1') {
            steps {
                script {
                    sh(script: "source /etc/profile; helm upgrade --set version=$SERVICE_1_VERSION_NUMBER p2-staging-service1-workflow $GITHUB_LINK/service1-workflow-0.1.0.tgz -f $GITHUB_LINK/service1-workflow/values.p2.staging.yaml")
                }
            }
        }
        stage('Project2 - Staging - Service 2') {
            steps {
                script {
                    sh(script: "source /etc/profile; helm upgrade --set version=$SERVICE_2_VERSION_NUMBER --set SERVICE1_URL=http://project2-staging-service1-svc:8080 p2-staging-service2-workflow $GITHUB_LINK/service2-workflow-0.1.0.tgz -f $GITHUB_LINK/service2-workflow/values.p2.staging.yaml")
                }
            }
        }
        stage('Project2 - Prod - Service 1') {
            steps {
                script {
                    sh(script: "source /etc/profile; helm upgrade --set version=$SERVICE_1_VERSION_NUMBER p2-prod-service1-workflow $GITHUB_LINK/service1-workflow-0.1.0.tgz -f $GITHUB_LINK/service1-workflow/values.p2.prod.yaml")
                }
            }
        }

        stage('Project2 - Prod - Service 2') {
            steps {
                script {
                    sh(script: "source /etc/profile; helm upgrade --set version=$SERVICE_2_VERSION_NUMBER --set SERVICE1_URL=http://project2-prod-service1-svc:8080 p2-prod-service2-workflow $GITHUB_LINK/service2-workflow-0.1.0.tgz -f $GITHUB_LINK/service2-workflow/values.p2.prod.yaml")
                }
            }
        }

    }
}