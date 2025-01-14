# Install Jenkins on DigitalOcean

## 🚀 Technologies Used
- **Jenkins**
- **Docker**
- **DigitalOcean**
- **Linux**

## 📝 Project Description
This project demonstrates how to set up and configure Jenkins on an Ubuntu server hosted on DigitalOcean. The key steps include:

1. **Creating an Ubuntu server** on DigitalOcean.
2. **Setting up Jenkins** by running it as a Docker container.
   - **Install Docker** : apt install docker.io
   - **Run Jenkins Container with Jenkins image** : docker run -d -p 8080:8080 -p 50000:50000 -v jenkins-home:/var/jenkins-home -v /var/run/docker.sock:/var/run/docker.sock jenkins/jenkins:lts
      - **-d** : Detach mode
      - **-p** : Port
      - **-v jenkins-home:/var/jenkins-home** : -v will create a docker volume to persist data
      - **-v /var/run/docker.sock:/var/run/docker.sock** : This command will mounted docker CLI to a Jenkins Container that make Docker CLI available in Jenkins Container
      - **jenkins/jenkins:lts** : Jenkin Image
3. **Initializing Jenkins** for use in build automation and CI/CD workflows.

# Create a CI Pipeline with Jenkinsfile (Freestyle, Pipeline, Multibranch Pipeline)
## 🚀 
- **Jenkins**
- **Docker**
- **Linux**
- **Git**
- **Java**
- **Maven**
