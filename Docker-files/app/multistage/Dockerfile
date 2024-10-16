# Stage 1: Build Stage
FROM openjdk:11 AS BUILD_IMAGE

# Install necessary packages
RUN apt update && apt install -y maven openssh-client

# Copy the SSH private key and set permissions
COPY id_rsa /root/.ssh/id_rsa
RUN chmod 600 /root/.ssh/id_rsa

# Copy the known_hosts file into the container
COPY known_hosts /root/.ssh/known_hosts

# Clone the repository using SSH
RUN git clone git@github.com:devopshydclub/vprofile-project.git
RUN cd vprofile-project && git checkout docker && mvn install

# Stage 2: Application Image
FROM tomcat:9-jre11

# Clean the webapps directory
RUN rm -rf /usr/local/tomcat/webapps/*

# Copy the built WAR file from the build stage
COPY --from=BUILD_IMAGE vprofile-project/target/vprofile-v2.war /usr/local/tomcat/webapps/ROOT.war

# Expose port 8080 and run Tomcat
EXPOSE 8080
CMD ["catalina.sh", "run"]

