<img width="2880" height="1800" alt="image" src="https://github.com/user-attachments/assets/bf73fce4-90ef-413e-8530-df92a0f60f35" />

# Jenkins + Docker + GitHub CI/CD Notes

### Create Jenkins Volume

```powershell
docker volume create jenkins_home
```


### Run Jenkins Container

```powershell
docker run -d `
  --name jenkins `
  -p 8080:8080 `
  -p 50000:50000 `
  -v jenkins_home:/var/jenkins_home `
  -v //var/run/docker.sock:/var/run/docker.sock `
  jenkins/jenkins:lts
```



### Verify Jenkins

```powershell
docker ps
```

 

### Open Jenkins

```text
http://localhost:8080
```

 

### Get Initial Password

```powershell
docker exec jenkins cat /var/jenkins_home/secrets/initialAdminPassword
```

 

### Enter Jenkins Container

```powershell
docker exec -u 0 -it jenkins bash
```

 

### Install Docker CLI

```bash
apt update
apt install docker.io -y
```

 

### Verify Docker

```bash
docker version
docker ps
```

 

## Jenkins Pipeline

### Jenkinsfile

```groovy
pipeline {

    agent any

    environment {
        IMAGE_NAME = "smartcraze/jenkins-node-app"
    }

    stages {

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $IMAGE_NAME .'
            }
        }

        stage('Deploy Container') {
            steps {

                sh '''
                    docker rm -f nodeapp || true

                    docker run -d \
                      --name nodeapp \
                      -p 3000:3000 \
                      $IMAGE_NAME
                '''
            }
        }
    }

    post {

        success {
            echo 'Pipeline completed successfully'
        }

        failure {
            echo 'Pipeline failed'
        }
    }
}
```

 

## Git Commands

### Initialize Git

```powershell
git init
```

### Add Files

```powershell
git add .
```

### Commit

```powershell
git commit -m "initial commit"
```

### Add Remote

```powershell
git remote add origin https://github.com/smartcraze/jenkins-node-app.git
```

### Push Code

```powershell
git branch -M main
git push -u origin main
```

 

## Jenkins Job Config

### Create Pipeline Job

```text
New Item → Pipeline
```

### SCM Setup

```text
Pipeline script from SCM
SCM: Git
Repository: https://github.com/smartcraze/jenkins-node-app.git
Branch: */main
Script Path: Jenkinsfile
```

 

## Docker Permission Fix

```powershell
docker exec -u 0 -it jenkins bash
```

```bash
chmod 666 /var/run/docker.sock
```

 

## Deployment Flow

```text
GitHub Push
    ↓
Jenkins Trigger
    ↓
Docker Build
    ↓
Container Deploy
```

 

## Useful Docker Commands

### Running Containers

```powershell
docker ps
```

### Docker Images

```powershell
docker images
```

### Stop Container

```powershell
docker stop container_name
```

### Remove Container

```powershell
docker rm -f container_name
```

### Build Image

```powershell
docker build -t image_name .
```

### Run Container

```powershell
docker run -d -p 3000:3000 image_name
```

### Remove Image

```powershell
docker rmi image_name
```

 

## Quick Revision

| Topic         | Purpose          |
|     - |      - |
| Jenkins       | CI/CD Server     |
| Docker        | Containers       |
| GitHub        | Source Code      |
| Jenkinsfile   | Pipeline as Code |
| Docker Socket | Docker Access    |
| Pipeline      | Automation Flow  |
