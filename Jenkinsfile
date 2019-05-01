pipeline {
    agent {
        node {
            label 'master'
        }
    }
  environment {
        TERRAFORM_CMD = 'docker run --network host -w /app -v `pwd`:/app hashicorp/terraform:light'
    }
  stages {
      stage('Checkout') {
      steps {
        checkout scm
      }
    }
    stage('pull latest light terraform image') {
            steps {
                sh  """
                    docker pull hashicorp/terraform:light
                    """
            }
        }
    stage('init') {
            steps {
                sh  """
                    ${TERRAFORM_CMD} init -backend=true -input=false
                    """
            }
        }
     stage('Approval') {
      steps {
        script {
          def userInput = input(id: 'confirm', message: 'Apply Terraform?', parameters: [ [$class: 'BooleanParameterDefinition', defaultValue: false, description: 'Apply terraform', name: 'confirm'] ])
        }
      }
    }
  }
}