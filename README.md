# Deploying 2048 Game with Docker on Amazon Elastic Beanstalk

This guide will walk you through the steps to deploy the popular 2048 game on Amazon Elastic Beanstalk using Docker. The game source code is provided by [gabrielecirulli/2048](https://github.com/gabrielecirulli/2048).

## Prerequisites

Before proceeding, ensure you have the following prerequisites installed and configured:

- Docker installed on your local machine.
- An AWS account with permissions to create resources in Elastic Beanstalk.
- AWS Command Line Interface (CLI) installed and configured with appropriate access keys.

## Steps

### 1. Dockerfile

Create a Dockerfile with the following content:

```Dockerfile
FROM ubuntu:22.04

RUN apt-get update && \
    apt-get install -y nginx zip curl && \
    echo "daemon off;" >>/etc/nginx/nginx.conf && \
    curl -o /var/www/html/master.zip -L https://codeload.github.com/gabrielecirulli/2048/zip/master && \
    cd /var/www/html/ && unzip master.zip && mv 2048-master/* . && rm -rf 2048-master master.zip

EXPOSE 80

CMD ["/usr/sbin/nginx", "-c", "/etc/nginx/nginx.conf"]
```
### 2. Build the Docker Image

Build the Docker image from the directory containing the Dockerfile:

```bash
docker build -t 2048-game .
```
### 3. Run the Docker Container Locally

Run the Docker container locally to ensure everything is working:

```bash
docker run -d -p 8080:80 2048-game
```
### 4. Set Up Elastic Beanstalk Environment

Navigate to the AWS Management Console and open the Elastic Beanstalk service.

- Click "Create New Application" and follow the prompts to create a new application.
- Choose "Docker" as the platform.
- Upload the Docker image you built earlier.

### 5. Configure Environment

Navigate to the AWS Management Console and open the Elastic Beanstalk service.

- Click "Create New Application" and follow the prompts to create a new application.
- Choose "Docker" as the platform.
- Upload the Docker image you built earlier.

### 6. Deploy Application

Click "Deploy" to deploy the application to Elastic Beanstalk.

### 7. Access the Game

Once the deployment is successful, Elastic Beanstalk will provide you with a URL to access the deployed game. Visit the URL in your browser to play 2048.
