``` bash
docker pull django
docker run  -p 8000:8000 -ti django:latest /bin/bash
apt-get install -y vim
django-admin startproject mysite
cd mysite
python manage.py runserver 0.0.0.0:8000
```
