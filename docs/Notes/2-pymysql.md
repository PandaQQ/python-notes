## 2 - PyMySQL with Django Project

> django.core.exceptions.ImproperlyConfigured: Error loading MySQLdb module. 解决方法



```
pip3 install PyMySQL

# Go to settings.py folder's
# __init__.py

import pymysql
pymysql.install_as_MySQLdb()
```



#### Summary

 相对而言，使用mysqlclient需要通过homebrew安装mysql而言。个人还是比较喜欢Pure Python Lib。