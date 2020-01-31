pipeline {
        environment {
            registryCredential ="#@#DOCKERHUB_CREDENTIAL#@#"
            dockerImage = ""
            registry = "#@#DOCKERHUB_REGISTRY#@#"
        }
        agent any
        stages {
            stage('Build & Publish new Docker images') {
                steps {
                    echo 'Starting to build docker image DB'
                    script {
                        dir ('db') {
                            dockerImage = docker.build(registry + ":db-${BUILD_ID}")
                            docker.withRegistry("https://registry.hub.docker.com", registryCredential){
                            dockerImage.push()
                           }
                         }
                        }
                    echo 'Starting to build docker image APP'
                    script {
                        dir ('app') {
                            dockerImage = docker.build(registry + ":app-${BUILD_ID}")
                            docker.withRegistry("https://registry.hub.docker.com", registryCredential){
                            dockerImage.push()
                           }
                         }
                        }
                    echo 'Starting to build docker image WEB'
                    script {
                        dir ('web') {
                            dockerImage = docker.build(registry + ":web-${BUILD_ID}")
                            docker.withRegistry("https://registry.hub.docker.com", registryCredential){
                            dockerImage.push()
                           }
                         }
                        }
                	}
             	    }
//            stage('Deploy') {
//                steps {
//                    sshCommand remote: remote, command: "sed -i 's/${BUILD_ID - 1}/${BUILD_ID}/g' /opt/docker/ansible/k8s-mattermost-deployment.yml"
//                    sshCommand remote: remote, command: "ansible-playbook k8s-mattermost-deployment.yml -u k8s-admin"
//                }
//            }
            stage('Cleanup') {
                steps {
                    sh "docker rmi '${registry}:db-${BUILD_ID}'"
                    sh "docker rmi '${registry}:app-${BUILD_ID}'"
                    sh "docker rmi '${registry}:web-${BUILD_ID}'"
                   }
                  }
         }
 }
