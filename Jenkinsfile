pipeline {
  agent any

  environment {
    AWS_REGION = 'us-east-1'
    EKS_CLUSTER_NAME = 'devi-eks'
  }

  stages {
    stage('Checkout') {
      steps {
        checkout scm
      }
    }

    stage('Echo') {
      steps {
        echo "✅ Jenkinsfile is working and checkout was successful."
      }
    }

    stage('List Files') {
      steps {
        sh 'ls -la'
      }
    }

    stage('Configure Kubeconfig') {
      steps {
        sh """
          aws sts get-caller-identity
          aws eks list-clusters --region ${AWS_REGION}
          aws eks update-kubeconfig \
            --region ${AWS_REGION} \
            --name ${EKS_CLUSTER_NAME}
        """
      }
    }

    stage('Deploy to EKS') {
      steps {
        sh 'kubectl apply -f deployment.yaml'
      }
    }

    stage('Verify Deployment') {
      steps {
        sh 'kubectl rollout status deployment/nginx'
      }
    }
  }

  post {
    failure {
      echo 'Deployment failed.'
    }
    success {
      echo 'Deployment succeeded.'
    }
  }
}
