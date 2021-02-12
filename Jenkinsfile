def list_projects = ["p1", "p2", "p3"]
def list_environments = ["staging", "prod"]
pipeline {
    agent any
    environment {
        SERVICE_1_VERSION_NUMBER = sh(script: 'cat service1_version.txt ', returnStdout: true).trim()
        SERVICE_2_VERSION_NUMBER = sh(script: 'cat service2_version.txt ', returnStdout: true).trim()
        GITHUB_LINK_S1 = "https://github.com/mabutkovic/service1/raw/master"
        GITHUB_LINK_S2 = "https://github.com/mabutkovic/service2/raw/master"
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
        stage('Deployment Stage') {

            steps {
                script {
                    for (int i = 0; i < list_projects.size(); i++) {
                        for (int j = 0; j < list_environments.size(); j++) {

                            stage(list_projects[i] + '-' + list_environments[j] + ' deploy') {
                                sh(script: "source /etc/profile; helm upgrade --install --set version=$SERVICE_1_VERSION_NUMBER " + list_projects[i] + "-" + list_environments[j] + "-service1-workflow $GITHUB_LINK_S1/service1-workflow-0.1.0.tgz -f $GITHUB_LINK_S1/service1-workflow/values." + list_projects[i] + "." + list_environments[j] + ".yaml")
                                sh(script: "source /etc/profile; helm upgrade --install --set version=$SERVICE_2_VERSION_NUMBER --set SERVICE1_URL=http://" + list_projects[i] + "-" + list_environments[j] + "-service1-svc:8080 " + list_projects[i] + "-" + list_environments[j] + "-service2-workflow $GITHUB_LINK_S2/service2-workflow-0.1.0.tgz -f $GITHUB_LINK_S2/service2-workflow/values." + list_projects[i] + "." + list_environments[j] + ".yaml")
                            }

                        }
                    }
                }
            }
        }
    }

}