# Use a base image
FROM openjdk:11 AS BUILD_IMAGE

# Install necessary packages
RUN apt update && apt install -y maven git openssh-client

# Create .ssh directory
RUN mkdir -p /root/.ssh

# Copy your SSH private key and known_hosts file into the image
# Make sure to have your SSH private key (id_rsa) and known_hosts in the build context
COPY id_rsa /root/.ssh/id_rsa
COPY known_hosts /root/.ssh/known_hosts

# Set permissions for the SSH key
RUN chmod 600 /root/.ssh/id_rsa
RUN chmod 644 /root/.ssh/known_hosts

# Clone the repository using SSH
RUN git clone git@github.com:devopshydclub/vprofile-project.git

# Change to the project directory and build the application
RUN cd vprofile-project && git checkout docker && mvn install

# Final image
FROM tomcat:9-jre11

# Remove existing web applications
RUN rm -rf /usr/local/tomcat/webapps/*

# Copy the WAR file from the build stage
COPY --from=BUILD_IMAGE /vprofile-project/target/vprofile-v2.war /usr/local/tomcat/webapps/ROOT.war

# Expose the application port
EXPOSE 8080

# Start Tomcat
CMD ["catalina.sh", "run"]
