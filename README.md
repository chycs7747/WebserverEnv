# WebserverEnv
### gunicorn_django  

- Add Dockerfile for containerizing Django application with Gunicorn  
- Django, gunicorn are installed in django_env (virtual environment)  

- gunicorn 0.0.0.0:8000 bind with Django 0.0.0.0:8000  
- gunicorn's configuration path : /app/conf/gunicorn_config.py  
- Django's settings.py path : /app/myproject/myproject/settings.py  
- Project Root Dir : myproject -> Dockerfile ENV DJANGO_SETTINGS_MODULE=myproject.settings
