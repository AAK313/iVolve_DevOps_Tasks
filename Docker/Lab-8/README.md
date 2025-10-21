# Lab 8: Run Java Spring Boot App in a Container

This lab demonstrates how to containerize and run a Java Spring Boot application using Docker.  
The Dockerfile copies the entire source code into the image and builds the application using Maven inside the container.

---

## 1. Clone the Application Code

```bash
git clone https://github.com/Ibrahim-Adel15/Docker-1.git
cd Docker-1
```
## 2.  Write Dockerfile

```bash
Create a Dockerfile in the project root and add the following commands.

Dockerfile Commands Explanation:

FROM maven:sapmachine → Use a Maven image with Java 17 (SapMachine variant) as the base image.

WORKDIR /app → Set the working directory inside the container to /app.

COPY . . → Copy all source code from the current directory on the host to /app in the container.

RUN mvn package → Build the application inside the container using Maven, producing a JAR file.

EXPOSE 8080 → Expose port 8080 so the app can receive traffic.

ENTRYPOINT ["java", "-jar", "target/demo-0.0.1-SNAPSHOT.jar"] → Run the Spring Boot application JAR when the container starts.
```

As we see this lab isn't best practise to add all source code to the container and adding a layer to build the code

## 3. Run the Container

Start a new container from the built image, mapping port 8080 on the host to port 8080 in the container.
```bash
docker run -d -p 8080:8080 --name app1-container app1-image
```
## 4. Test the Application

Check if the application is running by sending a request to port 8080.
``bash
curl http://localhost:8080
```

Or open in your browser:

http://localhost:8080

6. Stop and Remove the Container

Stop the running container and remove it from the system.
```bash
docker stop app1-container
docker rm app1-container
```

## 7. Verify Container and Logs

Check running containers and view application logs for troubleshooting.
```bash
docker ps
docker logs app1-container
```
