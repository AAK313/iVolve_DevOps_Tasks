
# Lab 9: Run Java Spring Boot App in a Container

## 1. Clone the Application Code

Clone the GitHub repository that contains the Spring Boot project.

```bash
git clone https://github.com/Ibrahim-Adel15/Docker-1.git
cd Docker-1
```
## 2. Build the JAR File

Use Maven to build the application locally on your machine.
```bash
mvn clean package
```

This command generates the JAR file located at:

target/demo-0.0.1-SNAPSHOT.jar

## 3. Write Dockerfile
```bash
Create a Dockerfile in the project root and include the following commands.

Dockerfile Commands Explanation:

FROM eclipse-temurin:17-jdk → Use Java 17 base image (Eclipse Temurin).

WORKDIR /app → Set working directory inside the container.

COPY target/demo-0.0.1-SNAPSHOT.jar app.jar → Copy the locally built JAR file into the container.

EXPOSE 8080 → Expose port 8080 for application access.

ENTRYPOINT ["java", "-jar", "app.jar"] → Run the Spring Boot JAR when the container starts.
```
## 4. Build Docker Image

Build the image and tag it as app2-image.
```bash
docker build -t app2-image .
```

Note the image size comparing to the image size of last lab.

## 5. Run the Container

Run a container from the newly built image, mapping port 8080 on your host to 8080 inside the container.
```bash
docker run -d -p 8080:8080 --name app2-container app2-image
```
## 6. Test the Application

Verify the app is running by accessing it via curl or a browser.
```bash
curl http://localhost:8080
```

Or open in your browser:

http://localhost:8080

## 7. Stop and Remove the Container

Stop the running container and remove it.
```bash
docker stop app2-container
docker rm app2-container
```
## 8. (Optional) Check Running Containers and Logs
```bash
docker ps
docker logs app2-container
```

