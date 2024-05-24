# WebserverEnv
### gunicorn_django  

Build image: `docker build -t myproject:latest .`

Run container: `docker run -d -p 8000:8000 myproject:latest`



To connect to Gunicorn through Nginx, configure `proxy_pass` to forward to port 8000 on `0.0.0.0`

If you need to modify the Gunicorn configuration, follow these steps:

1. Edit the contents of `/app/conf/gunicorn_config.py`.
2. If there is a running instance of Gunicorn, stop it using `pkill gunicorn`.
3. Restart Gunicorn with the command `gunicorn -c conf/gunicorn_config.py myproject.wsgi`.

If you need to change Django's settings, edit and save the contents of `/app/myproject/myproject/settings.py`.  



- Add Dockerfile for containerizing Django application with Gunicorn  
- Django, gunicorn are installed in django_env (virtual environment)  

- gunicorn 0.0.0.0:8000 bind with Django 0.0.0.0:8000  
- gunicorn's configuration path : /app/conf/gunicorn_config.py  
- Django's settings.py path : /app/myproject/myproject/settings.py  
- Project Root Dir : myproject -> Dockerfile ENV DJANGO_SETTINGS_MODULE=myproject.settings





p.s) If you use nginx,

1. `sudo vi /etc/nginx/sites-available/myproject`

2. ```
   server {
           listen 80;
           server_name 127.0.0.1; # server IP address
   
           location / {
                   proxy_pass http://0.0.0.0:8000; # Django's allowed host
           }
   }
   
   ```



## nginx_gunicorn_django

Docker Compose is used to define and run multi-container Docker applications.  

1. **Nginx**: Nginx acts as a reverse proxy server in this configuration.  
2. **Gunicorn**: Gunicorn serves as the WSGI (Web Server Gateway Interface). server for running Python applications.  
3. **Django**: Django is a high-level Python web framework that encourages rapid development and clean, pragmatic design.



**Usage Instructions:**

1. **Build the Image**
   - Use `docker compose build` to create the image. This command reads the `docker-compose.yaml` file and builds the images as specified in the configuration.
2. **Run the Containers**
   - Execute `docker compose up -d` to start the containers in detached mode. This will bring up the Docker environment according to the configurations defined in the Docker Compose file, running the containers in the background.

**Tips:**

- **Stopping and Removing Containers**
  - Use `docker compose down` to stop and remove the running containers along with the networks they use. This command is useful for cleaning up after you're done testing or running your application.
- **Rebuilding Images**
  - If you make changes to the files, you need to rebuild the images with `docker compose build` to apply these changes. After rebuilding, use `docker compose up -d` to restart the containers with the updated configurations and code.



#### Notice

**Django Settings (app1)**

- In the `settings.py` of the Django application, the `ALLOWED_HOSTS` variable is set to `['0.0.0.0', 'web']`. This configuration means that Django will accept requests from any IP address (`0.0.0.0`) and also specifically from a hostname called 'web', which is typically the service name defined in Docker Compose.  

  `ALLOWED_HOSTS = ['0.0.0.0', 'web'] `



**Gunicorn Configuration (app1)**

- The `gunicorn_config.py` file contains a setting `bind = '0.0.0.0:8000'`. This tells Gunicorn to bind to all IP addresses on the host machine, allowing it to accept requests on port 8000 from any network interface.  

  `bind = '0.0.0.0:8000'`



**Nginx Configuration (app2)**

- The `proxy_pass` directive within the Nginx configuration forwards requests to `http://web:8000;`. Here, `web` refers to the service name of the Django application as defined in Docker Compose. This setup ensures that Nginx acts as a reverse proxy, directing incoming requests to the Django application running under Gunicorn.  

  `proxy_pass http://web:8000;  # Use the service name as the host (current: docker-compose's service name)`



**Server Name in Nginx Configuration (app2)**

- In the Nginx configuration file, typically named something like `myproject.conf`, the `server_name` is set to an IP address, `172.25.235.100;`. This is the IP where Nginx is accessible and should be replaced with the actual IP address of your server where Nginx is running.  

  `server_name 172.25.235.100;  # Your server IP address (current: example IP address)`
