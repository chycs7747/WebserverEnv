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





example) If you use nginx,

1. `sudo vi /etc/nginx/sites-available/myproject.`

2. ```
   server {
           listen 80;
           server_name 127.0.0.1; # server IP address
   
           location / {
                   proxy_pass http://0.0.0.0:8000; # Django's allowed host
           }
   }
   
   ```



