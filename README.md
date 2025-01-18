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
   - **Install Docker** : `apt install docker.io`
   - **Run Jenkins Container with Jenkins image** : `docker run -d -p 8080:8080 -p 50000:50000 -v jenkins-home:/var/jenkins-home -v /var/run/docker.sock:/var/run/docker.sock jenkins/jenkins:lts`
      - **-d** : Detach mode
      - **-p** : Port
      - **-v jenkins-home:/var/jenkins-home** : -v will create a docker volume to persist data
      - **-v /var/run/docker.sock:/var/run/docker.sock** : This command will mounted docker CLI to a Jenkins Container that make Docker CLI available in Jenkins Container
      - **jenkins/jenkins:lts** : Jenkin Image
3. **Initializing Jenkins** for use in build automation and CI/CD workflows.

# Create a CI Pipeline with Jenkinsfile (Freestyle, Pipeline, Multibranch Pipeline)
## 🚀 Technologies Used
- **Jenkins**
- **Docker**
- **Linux**
- **Git**
- **Java**
- **Maven**

## 📝 Project Description
CI Pipline for a Java Maven application to build and push to the repository
1. **Install Build Tools** Maven in Jenkin
2. Make Docker available in Jenkin Server
3. Create **Jenkins Credentials** for a (git Repo, Docker hub)
4. Create **Jenkins Jobs Types** (Freestyle, Piplelines, Multibranch Pipline) for Java Maven project with Maven to:
   1. Connect to Applications's git Repo
   2. Build jar
   3. Build Docker Image
   4. Push to private Dockerhub Repo
  
## Jenkin pipelines syntax 
```
pipeline {
   agent any
    
    stages {

        stage("test") {
            steps {
               script {
                  echo "Testing Applications"
               }
            }
        }
    
        stage("build") {
            steps {
               
            }
        }

        stage("Deploy") {
            steps {
               
            }
        }
       
    } 
}
```
1. **pipelines** : 
2. **agen** : The agent section specifies where the entire Pipeline, or a specific stage, will execute in the Jenkins environment depending on where the agent section is placed. The section must be defined at the top-level inside the pipeline block, but stage-level usage is optional.
3. **stages**: Containing a sequence of one or more **stage** directives, the stages section is where the bulk of the "work" described by a Pipeline will be located. At a minimum, it is recommended that stages contain at least one stage directive for each discrete part of the continuous delivery process, such as Build, Test, and Deploy.
4. **steps**: The steps section defines a series of one or more steps to be executed in a given stage directive.
5. More Information of Jenkins Syntax : `https://www.jenkins.io/doc/book/pipeline/syntax/`

# Install Nexus as a Docker container
## 🚀 Technologies Used
- **Nexus**
- **Docker**
- **DigitalOcean**
- **Linux**

## 📝 Project Description
This project demonstrate how to deploy Nexus as a Docker container 
1. **Create an Ubuntu server** on Digital Ocean -
2. **Install docker** `apt install docker.io`
3. **Run Nexus container as a Nexus image**: 
   1. **Create Nexus volume** : `docker volume create --name nexus-data`
   2. **Run Nexus Image**: `docker run -d -p 8081:8081 -p 8083:8083 --name nexus -v nexus-data:/nexus-data sonatype/nexus3`
      1. **-d** : Detach mode
      2. **-p 8081:8081** : Nexus run on port 8081  
      3. **-p 8083:8083** : To connect with docker i have to create another port for that
      4. **-v nexus-data:/nexus-data** : Nexus volume to persist data
4. Because of Nexus run on **http** so I have to set up **Insecurity-Registy** in the Docker to accept http request 
   1. In Linux: Navigate to **/etc/docker/daemon.json**
   2. ```
      {

        "insecure-registries": ["my-insecure-registry.local:5000"]

       }
   ```
 5. **docker.sock** : is the Unix socket file Docker used to communicate with Docker client **/var/run/docker.sock**
 6. In Server we should create another user to run Nexus as a Docker container (never run with root user)
    1. Create new user : `adduser username`
    2. Add that user to Sudo Group so it can have a root privilege : `chmod -aG sudo username`
    3. Make docker CLI available for all user : **for group** `chmod g=rw /var/run/docker.sock` **for other** `chmod o=rw /var/run/docker.sock`
# Push Docker Image to Nexus
## In Nexus 
1. Create Docker Repository
   1. Open port 8083 to connect with Docker
2. Create Docker Blob Store 
3. Create **Role** and **User**
4. Set that **User** to Docker-hosted **Role**
5. Open Docker Authencation
## Build and Push Docker image to Nexus 
1. **Create Maven/Gradle/NPM artifact** : `mvn package` or `gradle build` or `npm pack`
2. **Build Image**: `docker build -t *nexus-ipaddress:port:app-name-version* .`
3. **Login docker to Nexus**: `docker login nexus-ipAddress:port`
4. **Push Docker to Nexus**: `docker push *nexus-ipaddress:port:app-name-version*`
