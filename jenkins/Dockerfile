# Use the official Jenkins LTS image as the base image
FROM jenkins/jenkins:lts

# Switch to the root user to install additional packages
USER root

# Install any necessary packages (optional)
# For example, to install sudo and Docker CLI:
RUN apt-get update && \
    apt-get install -y sudo docker.io && \
    apt-get clean

# Ensure the Jenkins user can use Docker
RUN usermod -aG docker jenkins

# Switch back to the Jenkins user
USER jenkins
