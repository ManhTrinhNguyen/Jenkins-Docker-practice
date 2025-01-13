Install Jenkins on DigitalOcean

Technologies Used

Jenkins

Docker

DigitalOcean

Linux

Project Description

This project demonstrates how to:

Create an Ubuntu server on DigitalOcean.

Set up and run Jenkins as a Docker container.

Initialize Jenkins for build automation and continuous integration/continuous delivery (CI/CD).

Steps to Reproduce

1. Create an Ubuntu Server on DigitalOcean

Log in to your DigitalOcean account.

Create a droplet using the Ubuntu image (latest LTS version).

Configure the server with appropriate specifications.

Access the server via SSH using the provided IP address.

2. Set Up and Run Jenkins as a Docker Container

Install Docker on the Ubuntu server:

sudo apt update
sudo apt install docker.io -y
sudo systemctl start docker
sudo systemctl enable docker

Pull the Jenkins Docker image:

docker pull jenkins/jenkins:lts

Run the Jenkins container:

docker run -d \
  --name jenkins \
  -p 8080:8080 -p 50000:50000 \
  -v jenkins_home:/var/jenkins_home \
  jenkins/jenkins:lts

3. Initialize Jenkins

Access Jenkins via http://<your-server-ip>:8080 in your browser.

Follow the setup wizard:

Enter the initial admin password from the file /var/jenkins_home/secrets/initialAdminPassword inside the container:

docker exec jenkins cat /var/jenkins_home/secrets/initialAdminPassword

Install recommended plugins.

Create an admin user.

Configure Jenkins as needed for your CI/CD pipelines.

Module Reference

This project is part of Module 8: Build Automation & CI/CD with Jenkins, focusing on:

Automating build processes.

Setting up CI/CD pipelines with Jenkins.

Leveraging Docker for containerized Jenkins deployments.

