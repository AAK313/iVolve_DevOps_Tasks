# Lab 12: Docker Volume and Bind Mount with Nginx

## 🎯 Objective
Learn how to use **Docker Volumes** and **Bind Mounts** to persist data and serve custom content with an Nginx container.

---

## 🧩 Steps

### 1️⃣ Create a Docker Volume
Create a named volume called `nginx_logs` to persist Nginx log files:
```bash
docker volume create nginx_logs
Verify the volume is created:

bash
Copy code
docker volume ls
You can also check its path:

bash
Copy code
docker volume inspect nginx_logs
2️⃣ Create Bind Mount Directory and HTML File
Create a directory on your host to store the custom HTML content:

bash
Copy code
mkdir -p nginx-bind/html
Create an index.html file inside it:

bash
Copy code
echo "Hello from Bind Mount" > nginx-bind/html/index.html
3️⃣ Run Nginx Container with Volume and Bind Mount
Run the Nginx container with:

Volume for /var/log/nginx

Bind Mount for /usr/share/nginx/html

Port mapping 8085:80

bash
Copy code
docker run -d --name nginx-lab12 \
-p 8085:80 \
-v nginx_logs:/var/log/nginx \
-v $(pwd)/nginx-bind/html:/usr/share/nginx/html \
nginx
Verify that the container is running:

bash
Copy code
docker ps
4️⃣ Test Nginx Page
Use the curl command to verify the web page:

bash
Copy code
curl http://localhost:8085
✅ You should see:

csharp
Copy code
Hello from Bind Mount
5️⃣ Modify the HTML File and Verify Changes
Update the content in the index.html file:

bash
Copy code
echo "Updated from Local Machine!" > nginx-bind/html/index.html
Re-run the test:

bash
Copy code
curl http://localhost:8085
✅ The output should now display:

sql
Copy code
Updated from Local Machine!
6️⃣ Verify Logs Persist in the Volume
Access the container shell:

bash
Copy code
docker exec -it nginx-lab12 bash
Check log files inside the container:

bash
Copy code
ls /var/log/nginx
You can also verify from your host:

bash
Copy code
docker volume inspect nginx_logs
Then navigate to the mount point and confirm the logs exist.

7️⃣ Clean Up Resources
Stop and remove the container:

bash
Copy code
docker stop nginx-lab12
docker rm nginx-lab12
Remove the volume:

bash
Copy code
docker volume rm nginx_logs
