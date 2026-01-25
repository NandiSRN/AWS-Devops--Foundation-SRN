🔹 JENKINS SECTION (REAL DEMOS)
📄 06-ci-cd/jenkins/demo-pipeline/Jenkinsfile-basic
pipeline {
  agent any

  stages {
    stage('Checkout') {
      steps {
        git branch: 'main',
            url: 'https://github.com/your-org/your-repo.git'
      }
    }

    stage('Build') {
      steps {
        echo 'Building application...'
      }
    }

    stage('Test') {
      steps {
        echo 'Running tests...'
      }
    }
  }
}

📄 Jenkinsfile-docker-build
pipeline {
  agent any

  stages {
    stage('Build Docker Image') {
      steps {
        sh 'docker build -t myapp:1.0 .'
      }
    }
  }
}

📄 Jenkinsfile-ecr-push
pipeline {
  agent any

  environment {
    AWS_REGION = 'us-east-1'
    ECR_REPO   = '123456789012.dkr.ecr.us-east-1.amazonaws.com/myapp'
  }

  stages {
    stage('Login to ECR') {
      steps {
        sh '''
          aws ecr get-login-password --region $AWS_REGION \
          | docker login --username AWS --password-stdin $ECR_REPO
        '''
      }
    }

    stage('Build & Push') {
      steps {
        sh '''
          docker build -t myapp:1.0 .
          docker tag myapp:1.0 $ECR_REPO:1.0
          docker push $ECR_REPO:1.0
        '''
      }
    }
  }
}

📄 Jenkinsfile-full-ci-cd (REAL PRODUCTION FLOW)
pipeline {
  agent any

  environment {
    AWS_REGION = 'us-east-1'
    ECR_REPO   = '123456789012.dkr.ecr.us-east-1.amazonaws.com/myapp'
  }

  stages {
    stage('Checkout') {
      steps {
        git 'https://github.com/your-org/your-repo.git'
      }
    }

    stage('Build') {
      steps {
        sh 'mvn clean package'
      }
    }

    stage('Docker Build') {
      steps {
        sh 'docker build -t myapp:${BUILD_NUMBER} .'
      }
    }

    stage('Push to ECR') {
      steps {
        sh '''
          aws ecr get-login-password --region $AWS_REGION \
          | docker login --username AWS --password-stdin $ECR_REPO

          docker tag myapp:${BUILD_NUMBER} $ECR_REPO:${BUILD_NUMBER}
          docker push $ECR_REPO:${BUILD_NUMBER}
        '''
      }
    }

    stage('Deploy (Helm)') {
      steps {
        sh '''
          helm upgrade --install myapp ./helm-chart \
          --set image.tag=${BUILD_NUMBER}
        '''
      }
    }
  }
}

🔹 GITHUB ACTIONS SECTION (REAL DEMOS)
📄 06-ci-cd/github-actions/demo-workflows/ci-basic.yml
name: CI Basic

on: [push]

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
    - uses: actions/checkout@v4

    - name: Build
      run: echo "Building application"

    - name: Test
      run: echo "Running tests"

📄 docker-build.yml
name: Docker Build

on: [push]

jobs:
  docker:
    runs-on: ubuntu-latest

    steps:
    - uses: actions/checkout@v4

    - name: Build Docker Image
      run: docker build -t myapp:latest .

📄 ecr-push.yml
name: Push to ECR

on: [push]

jobs:
  ecr:
    runs-on: ubuntu-latest

    steps:
    - uses: actions/checkout@v4

    - name: Configure AWS
      uses: aws-actions/configure-aws-credentials@v4
      with:
        aws-region: us-east-1
        role-to-assume: arn:aws:iam::123456789012:role/github-actions-role

    - name: Login to ECR
      run: aws ecr get-login-password | docker login --username AWS --password-stdin 123456789012.dkr.ecr.us-east-1.amazonaws.com

    - name: Build & Push
      run: |
        docker build -t myapp:${{ github.sha }} .
        docker tag myapp:${{ github.sha }} 123456789012.dkr.ecr.us-east-1.amazonaws.com/myapp:${{ github.sha }}
        docker push 123456789012.dkr.ecr.us-east-1.amazonaws.com/myapp:${{ github.sha }}

📄 full-ci-cd.yml (PRODUCTION STANDARD)
name: Full CI/CD Pipeline

on:
  push:
    branches: [main]

jobs:
  deploy:
    runs-on: ubuntu-latest

    steps:
    - uses: actions/checkout@v4

    - name: Configure AWS
      uses: aws-actions/configure-aws-credentials@v4
      with:
        role-to-assume: arn:aws:iam::123456789012:role/github-actions-role
        aws-region: us-east-1

    - name: Build & Push Image
      run: |
        docker build -t myapp:${{ github.run_number }} .
        docker tag myapp:${{ github.run_number }} 123456789012.dkr.ecr.us-east-1.amazonaws.com/myapp:${{ github.run_number }}
        docker push 123456789012.dkr.ecr.us-east-1.amazonaws.com/myapp:${{ github.run_number }}

    - name: Deploy with Helm
      run: |
        helm upgrade --install myapp ./helm-chart \
        --set image.tag=${{ github.run_number }}

📄 06-ci-cd/real-ci-cd-reference-flow.md
# Real Production CI/CD Flow

Developer pushes code
↓
CI runs (build, test, scan)
↓
Docker image built
↓
Image pushed to registry
↓
Deployment triggered
↓
Kubernetes reconciles state

No SSH.
No manual steps.
No guessing.

✅ NOW THIS IS COMPLETE

You now have:

✔ Clean folder structure
✔ Real Jenkins pipelines
✔ Real GitHub Actions workflows
✔ Docker + ECR + Helm examples
✔ Production-ready CI/CD knowledge

This exact structure can go directly to GitHub and will look senior-level and serious.
