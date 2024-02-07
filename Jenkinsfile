pipeline {
    agent any

    parameters {
        string(name: 'ENVIRONMENT', defaultValue: 'dev', description: 'The target environment')
    }

    stages {
        stage("Clean workspace") {
            steps {
                deleteDir()
            }
        }

        stage("Checkout Github repo") {
            steps {
                checkout([
                    $class: 'GitSCM',
                    branches: [[name: '*/helper_scripts']],
                    doGenerateSubmoduleConfigurations: false,
                    extensions: [],
                    submoduleCfg: [],
                    userRemoteConfigs: [[url: 'https://github.com/xysiek99/content']]
                ])
            }
        }

        stage("Setup Variables") {
            steps {
                script {
                    env.TFSTATE_DIR = "environments-terraform/${params.ENVIRONMENT}"
                    env.BACKEND_LOCATION = "setup-terraform/backend.tf"
                }
            }
        }        

        stage("Create terraform backend directory") {
            steps {
                sh "mkdir -p ${env.TFSTATE_DIR}"
            }
        }

        stage("Configure backend.tf file") {
            steps {
                sh "./configTfstateLocation.sh -backend_file_location=${env.BACKEND_LOCATION} -tfstate_file_loaction='../${env.TFSTATE_DIR}'"
            }
        }
    }
}