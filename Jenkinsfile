pipeline {
    agent { label 'do-agent' }

    environment {
        core_banking_service = "core-banking-service"
        internet_banking_api_gateway = "internet-banking-api-gateway"
        internet_banking_config_server = "internet-banking-config-server"
        internet_banking_fund_transfer_service = "internet-banking-fund-transfer-service"
        internet_banking_service_registry = "internet-banking-service-registry"
        internet_banking_user_service = "internet-banking-user-service"
        internet_banking_utility_payment_service = "internet-banking-utility-payment-service"
    }

    stages {
        stage('Checkout') {
            steps {
                script {
                    checkout scm
                }
            }
        }

        stage('Detect changes') {
            steps {
                script {
                    def core_banking_service_changed = sh(script: "git diff --name-only HEAD~1 HEAD | grep '${core_banking_service}' || true", returnStdout: true).trim()
                    def internet_banking_api_gateway_changed = sh(script: "git diff --name-only HEAD~1 HEAD | grep '${internet_banking_api_gateway}' || true", returnStdout: true).trim()
                    def internet_banking_config_server_changed = sh(script: "git diff --name-only HEAD~1 HEAD | grep '${internet_banking_config_server}' || true", returnStdout: true).trim()
                    def internet_banking_fund_transfer_service_changed = sh(script: "git diff --name-only HEAD~1 HEAD | grep '${internet_banking_fund_transfer_service}' || true", returnStdout: true).trim()
                    def internet_banking_service_registry_changed = sh(script: "git diff --name-only HEAD~1 HEAD | grep '${internet_banking_service_registry}' || true", returnStdout: true).trim()
                    def internet_banking_user_service_changed = sh(script: "git diff --name-only HEAD~1 HEAD | grep '${internet_banking_user_service}' || true", returnStdout: true).trim()
                    def internet_banking_utility_payment_service_changed = sh(script: "git diff --name-only HEAD~1 HEAD | grep '${internet_banking_utility_payment_service}' || true", returnStdout: true).trim()

                    env.CORE_BANKING_SERVICE_CHANGED = core_banking_service_changed ? 'true' : 'false'
                    env.INTERNET_BANKING_API_GATEWAY_CHANGED = internet_banking_api_gateway_changed ? 'true' : 'false'
                    env.INTERNET_BANKING_CONFIG_SERVER_CHANGED = internet_banking_config_server_changed ? 'true' : 'false'
                    env.INTERNET_BANKING_FUND_TRANSFER_SERVICE_CHANGED = internet_banking_fund_transfer_service_changed ? 'true' : 'false'
                    env.INTERNET_BANKING_SERVICE_REGISTRY_CHANGED = internet_banking_service_registry_changed ? 'true' : 'false'
                    env.INTERNET_BANKING_USER_SERVICE_CHANGED = internet_banking_user_service_changed ? 'true' : 'false'
                    env.INTERNET_BANKING_UTILITY_PAYMENT_SERVICE_CHANGED = internet_banking_utility_payment_service_changed ? 'true' : 'false'
                }
            }
        }

        stage('Start Notification') {
            steps {
                script {
                    sh "echo 'Starting the build process...'"
                }
            }
        }

        stage('Code scan') {
            steps {
                script {
                    sh "echo 'Running code scan...'"
                }
            }
        }

        stage('filesystem scan') {
            steps {
                script {
                    sh "echo 'Running filesystem scan...'"
                }
            }
        }

        stage('Build docker image for core-banking-service') {
            when {
                expression { return env.CORE_BANKING_SERVICE_CHANGED == 'true' }
            }
            steps {
                dir('core-banking-service') {
                    script {
                        sh """
                        docker build -t ghcr.io/rohitdarekar816/core-banking-service:latest .
                        docker run --rm -v /var/run/docker.sock:/var/run/docker.sock dslim/slim build ghcr.io/rohitdarekar816/core-banking-service:latest
                        docker tag ghcr.io/rohitdarekar816/core-banking-service.slim:latest ghcr.io/rohitdarekar816/core-banking-service:slim
                        docker push ghcr.io/rohitdarekar816/core-banking-service:slim
                        """
                    }   
                }
            }
        }

        stage('Buidl docker image for internet-banking-api-gateway') {
            when {
                expression { return env.INTERNET_BANKING_API_GATEWAY_CHANGED == 'true'}
            }
            steps {
                dir('internet-banking-api-gateway') {
                    script {
                        sh """
                        docker build -t ghcr.io/rohitdarekar816/internet-banking-api-gateway:latest .
                        docker run --rm -v /var/run/docker.sock:/var/run/docker.sock dslim/slim build ghcr.io/rohitdarekar816/internet-banking-api-gateway:latest
                        docker tag ghcr.io/rohitdarekar816/internet-banking-api-gateway.slim:latest ghcr.io/rohitdarekar816/internet-banking-api-gateway:slim
                        docker push ghcr.io/rohitdarekar816/internet-banking-api-gateway:slim
                        """
                    }
                }
            }
        }

        stage('Build docker image for internet-banking-config-server') {
            when {
                expression { return env.INTERNET_BANKING_CONFIG_SERVER_CHANGED == 'true' }
            }
            steps {
                dir('internet-banking-config-server') {
                    script {
                        sh """
                        docker build -t ghcr.io/rohitdarekar816/internet-banking-config-server:latest .
                        docker run --rm -v /var/run/docker.sock:/var/run/docker.sock dslim/slim build ghcr.io/rohitdarekar816/internet-banking-config-server:latest
                        docker tag ghcr.io/rohitdarekar816/internet-banking-config-server.slim:latest ghcr.io/rohitdarekar816/internet-banking-config-server:slim
                        docker push ghcr.io/rohitdarekar816/internet-banking-config-server:slim
                        """
                    }
                }
            }
        }

        stage('Build docker image for internet-banking-fund-transfer-service') {
            when {
                expression { return env.INTERNET_BANKING_FUND_TRANSFER_SERVICE_CHANGED == 'true' }
            }
            steps {
                dir('internet-banking-fund-transfer-service') {
                    script {
                        sh """
                        docker build -t ghcr.io/rohitdarekar816/internet-banking-fund-transfer-service:latest .
                        docker run --rm -v /var/run/docker.sock:/var/run/docker.sock dslim/slim build ghcr.io/rohitdarekar816/internet-banking-fund-transfer-service:latest
                        docker tag ghcr.io/rohitdarekar816/internet-banking-fund-transfer-service.slim:latest ghcr.io/rohitdarekar816/internet-banking-fund-transfer-service:slim
                        docker push ghcr.io/rohitdarekar816/internet-banking-fund-transfer-service:slim
                        """
                    }
                }
            }
        }

        stage('Build docker image for internet-banking-service-registry') {
            when {
                expression { return env.INTERNET_BANKING_SERVICE_REGISTRY_CHANGED == 'true' }
            }
            steps {
                dir('internet-banking-service-registry') {
                    script {
                        sh """
                        docker build -t ghcr.io/rohitdarekar816/internet-banking-service-registry:latest .
                        docker run --rm -v /var/run/docker.sock:/var/run/docker.sock dslim/slim build ghcr.io/rohitdarekar816/internet-banking-service-registry:latest
                        docker tag ghcr.io/rohitdarekar816/internet-banking-service-registry.slim:latest ghcr.io/rohitdarekar816/internet-banking-service-registry:slim
                        docker push ghcr.io/rohitdarekar816/internet-banking-service-registry:slim
                        """
                    }
                }
            }
        }

        stage('Build docker image for internet-banking-user-service') {
            when {
                expression { return env.INTERNET_BANKING_USER_SERVICE_CHANGED == 'true' }
            }
            steps {
                dir('internet-banking-user-service') {
                    script {
                        sh """
                        docker build -t ghcr.io/rohitdarekar816/internet-banking-user-service:latest .
                        docker run --rm -v /var/run/docker.sock:/var/run/docker.sock dslim/slim build ghcr.io/rohitdarekar816/internet-banking-user-service:latest
                        docker tag ghcr.io/rohitdarekar816/internet-banking-user-service.slim:latest ghcr.io/rohitdarekar816/internet-banking-user-service:slim
                        docker push ghcr.io/rohitdarekar816/internet-banking-user-service:slim
                        """
                    }
                }
            }
        }

        stage('Build docker image for internet-banking-utility-payment-service') {
            when {
                expression { return env.INTERNET_BANKING_UTILITY_PAYMENT_SERVICE_CHANGED == 'true' }
            }
            steps {
                dir('internet-banking-utility-payment-service') {
                    script {
                        sh """
                        docker build -t ghcr.io/rohitdarekar816/internet-banking-utility-payment-service:latest .
                        docker run --rm -v /var/run/docker.sock:/var/run/docker.sock dslim/slim build ghcr.io/rohitdarekar816/internet-banking-utility-payment-service:latest
                        docker tag ghcr.io/rohitdarekar816/internet-banking-utility-payment-service.slim:latest ghcr.io/rohitdarekar816/internet-banking-utility-payment-service:slim
                        docker push ghcr.io/rohitdarekar816/internet-banking-utility-payment-service:slim
                        """
                    }
                }
            }
        }
    }
    post {
        success {
            script {
                sh "echo 'Build completed successfully!'"
            }
        }
        failure {
            script {
                sh "echo 'Build failed. Please check the logs.'"
            }
        }
        unstable {
            script {
                sh "echo 'Build is unstable. Please investigate.'"
                }
            }
        }
}
