# Lab 12: Docker Volume and Bind Mount with Nginx

## Objective
Learn how to use **Docker Volumes** and **Bind Mounts** to persist data and serve custom content with an Nginx container.

---

##  Steps

### 1-Create a Docker Volume
Create a named volume called `nginx_logs` to persist Nginx log files:
```bash
docker volume create nginx_logs
```
Verify the volume is created:
```
docker volume ls
```
You can also check its path:

```docker volume inspect nginx_logs
```
### 2-Create Bind Mount Directory and HTML File
Create a directory on your host to store the custom HTML content:

```
mkdir -p nginx-bind/html
```
Create an index.html file inside it:
```
echo "Hello from Bind Mount" > nginx-bind/html/index.html
```
### 3-Run Nginx Container with Volume and Bind Mount
Run the Nginx container with:

Volume for /var/log/nginx

Bind Mount for /usr/share/nginx/html

Port mapping 8085:80

```
docker run -d --name nginx-lab12 \
-p 8085:80 \
-v nginx_logs:/var/log/nginx \
-v $(pwd)/nginx-bind/html:/usr/share/nginx/html \
nginx
```
Verify that the container is running:

```
docker ps
```
### 4-Test Nginx Page
Use the curl command to verify the web page:
```
curl http://localhost:8085
```
### You should see:

```
Hello from Bind Mount
```
### 5-Modify the HTML File and Verify Changes
Update the content in the index.html file:
```
echo "Updated from Local Machine!" > nginx-bind/html/index.html
```
Re-run the test:

```
curl http://localhost:8085
```
## The output should now display:
```
Updated from Local Machine!
```
### 6-Verify Logs Persist in the Volume
Access the container shell:
```
docker exec -it nginx-lab12 bash
```
Check log files inside the container:
```
ls /var/log/nginx
```
You can also verify from your host:
```
docker volume inspect nginx_logs
```
Then navigate to the mount point and confirm the logs exist.

### 7-Clean Up Resources
Stop and remove the container:
```
docker stop nginx-lab12
docker rm nginx-lab12
```
Remove the volume:
```
docker volume rm nginx_logs
```
