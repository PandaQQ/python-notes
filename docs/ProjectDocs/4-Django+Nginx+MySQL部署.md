# 4-Django + Nginx + MSQL 部署教程





```
docker run -it --name web \
-v /Users/qzheng/PycharmProjects/medium:/root/medium \
python:3.11.1-slim-bullseye bash
```



```
/Users/qzheng/PycharmProjects/ecampus_site/venv/bin/daphne -b ecampus_site.asgi:application
```





```
docker run -d --name web \
-v /Users/qzheng/PycharmProjects/ecampus_site:/home/web \
python:3.10.10-slim-bullseye
```





```
docker run -d --name web \
-v /Users/qzheng/PycharmProjects/ecampus_site:/web \
-p 8000:8000 \
--restart=always \
pandaqq/django-python:1.0.0

```

