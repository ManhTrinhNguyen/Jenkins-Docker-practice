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

## Jenkin Incremental Version 
1. **This command is to automatic increase version in pom.xml** : `mvn build-helper:parse-version version:set -DnewVersion=\${parsedVersion.majorVersion}.\${parsedVersion.minorVersion}.\${parsedVersion.nextIncrementalVersion} versions:commit`
   1. **build-helper:parse-version** : Is the plugin . Parse the version inside the pom file
   2. **versions:set** : Set the version between version tag
   3. **-DnewVersion** : To set new version
   4. **\${parsedVersion.majorVersion}** : set the major version . If. i want to increase major version i will use \${parsedVersion.nextMajorVersion}
   5. **\${parsedVersion.minorVersion}** : set the minor version . If. i want to increase minor version i will use \${parsedVersion.nextMinorVersion}
   6. **versions:commit**  : To remove the old pom.xml version
**In Jenkinsfile**
1. **Create version increment stage** : Have to be before the build stage
2. **Added the increase version command** : `sh "mvn build-helper:parse-version version:set -DnewVersion=\\\${parsedVersion.majorVersion}.\\\${parsedVersion.minorVersion}.\\\${parsedVersion.nextIncrementalVersion} versions:commit"`
3. **Take the version from pom.xml** :
```def matcher = readFile(pom.xml) =~ <version>(.+)</version>
   def version = matcher[0][1]
   env.IMAGE_NAME="$version-$BUILD_NUMBER"
```
   - **readFile(pom.xml)** : Will read the pom.xml file-
   -  **=~ <version>(.+)</version>** : From pom.xml will find the version tag with its value
   - **matcher**: Will return a array of version tag
   - **matcher[0][1]**: Extract that value from version array
   - **env.IMAGE_NAME** : Create then save IMAGE_NAME variable to enviroment
   - **$BUILD_NUMBER** : is the build pipeline that from Jenkins Env
5. **In build stage** : To also update Docker Image version :
```
   sh "docker build -t docker-hub-repo:$IMAGE_NAME"
   sh "docker login -u username -p password" (No need to provide url to docker hub repo if other repo I need to provide address to that)
   sh "docker push docker-hub-repo:$IMAGE_NAME"
```

