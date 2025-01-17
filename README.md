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
