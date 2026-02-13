pipeline {
  agent any

  environment {
    DOCKERHUB_CREDS = 'dockerhub-creds'
    IMAGE_NAME = '2022bcs0056-jenkins'   // change this
  }

  stages {
    stage('Checkout') {
      steps { checkout scm }
    }

    stage('Install & Test/Train') {
      steps {
        sh '''
          python3 --version || true
          # if you use venv:
          python3 -m venv .venv
          . .venv/bin/activate
          pip install --upgrade pip
          pip install -r requirements.txt

          # run your training/eval script from Lab 4
          python scripts/train.py
        '''
      }
    }

    stage('Print Metrics + Sree Vardhan Reddy 2022BCS0056') {
      steps {
        sh '''
          echo "===== MODEL METRICS OUTPUT ====="
          cat metrics.json
          # IMPORTANT per lab: also print name + rollno
          echo "Name: Sree Vardhan Reddy"
          echo "RollNo: 2022BCS0056"
        '''
      }
    }

    stage('Build Docker Image') {
      steps {
        sh '''
          docker build -t $IMAGE_NAME:latest .
        '''
      }
    }

    stage('Push to DockerHub') {
      steps {
        withCredentials([usernamePassword(credentialsId: "${DOCKERHUB_CREDS}", usernameVariable: 'DH_USER', passwordVariable: 'DH_PASS')]) {
          sh '''
            echo "$DH_PASS" | docker login -u "$DH_USER" --password-stdin
            docker push $IMAGE_NAME:latest
          '''
        }
      }
    }
  }
}
