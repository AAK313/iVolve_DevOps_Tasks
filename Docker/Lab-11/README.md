Lab 11: Managing Docker Environment Variables Across Build and Runtime
Objective

Learn how to manage and pass environment variables in Docker during build time and runtime using different methods (direct command, environment file, and Dockerfile).

Steps
1. Clone the Application Code
git clone https://github.com/Ibrahim-Adel15/Docker-3.git
cd Docker-3

2. Write the Dockerfile

Create a file named Dockerfile in the project directory and add the following:

# Use Python base image
FROM python:3.10

# Set environment variables (for production environment)
ENV APP_MODE=production
ENV APP_REGION=canada-west

# Set working directory
WORKDIR /app

# Copy application code
COPY . .

# Install Flask
RUN pip install flask

# Expose port 8080
EXPOSE 8080

# Run the Flask application
CMD ["python", "app.py"]

3. Build the Docker Image
docker build -t app-env-demo .

4. Run Container with Environment Variables (Method 1 — Command Line)

Run the container and set environment variables manually:

docker run -d --name app-env1 -p 8081:8080 -e APP_MODE=development -e APP_REGION=us-east app-env-demo


Check if the container is running:

docker ps


Test the application:

curl http://localhost:8081

5. Run Container with Environment Variables (Method 2 — Environment File)

Create a new file named .env (or env.list) in the project directory:

APP_MODE=staging
APP_REGION=us-west


Run the container using the environment file:

docker run -d --name app-env2 -p 8082:8080 --env-file .env app-env-demo


Test the application:

curl http://localhost:8082

6. Run Container with Environment Variables Defined in Dockerfile

The Dockerfile already defines:

ENV APP_MODE=production
ENV APP_REGION=canada-west


Run the container without passing any variables:

docker run -d --name app-env3 -p 8083:8080 app-env-demo


Test the application:

curl http://localhost:8083
