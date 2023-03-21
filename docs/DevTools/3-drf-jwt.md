

```
Packages:
- djangorestframework-simplejwt;
- ldap;
```



```
'''
Django-auth-ldap 配置部分
'''

import ldap
from django_auth_ldap.config import LDAPSearch, GroupOfNamesType

# 修改Django认证先走ldap，再走本地认证
AUTHENTICATION_BACKENDS = [
    'django_auth_ldap.backend.LDAPBackend',
    'django.contrib.auth.backends.ModelBackend',
]
# ldap的连接基础配置
# ldap or ad 服务器地址
AUTH_LDAP_SERVER_URI = "ldap://auth-dc.hsu.edu.hk"
# 管理员的dn路径
AUTH_LDAP_BIND_DN = ""
# 管理员密码
AUTH_LDAP_BIND_PASSWORD = ""
# 允许认证用户的路径
AUTH_LDAP_USER_SEARCH = LDAPSearch(
    "OU=HS Users,DC=hsu,DC=edu,DC=hk",
    ldap.SCOPE_SUBTREE,
    "(&(objectClass=user)(sAMAccountName=%(user)s)(!(userAccountControl:1.2.840.113556.1.4.803:=2)))"
)

# 如果ldap服务器是Windows的AD，需要配置上如下选项

AUTH_LDAP_CONNECTION_OPTIONS = {
    ldap.OPT_DEBUG_LEVEL: 1,
    ldap.OPT_REFERRALS: 0,
}
# 当ldap用户登录时，从ldap的用户属性对应写到django的user数据库，键为django的属性，值为ldap用户的属性
AUTH_LDAP_USER_ATTR_MAP = {
    "first_name": "givenName",
    "last_name": "sn",
    "email": "mail"
}

'''
REST API FRAMEWORK WITH JWT
'''
import rest_framework
REST_FRAMEWORK = {
    'DEFAULT_FILTER_BACKENDS': [
        'django_filters.rest_framework.DjangoFilterBackend'
    ],
    'DEFAULT_AUTHENTICATION_CLASSES': [
        'rest_framework_simplejwt.authentication.JWTAuthentication',
    ],
}
```



#### [How to decode and verify simple-jwt-django-rest-framework token](https://stackoverflow.com/questions/62877088/how-to-decode-and-verify-simple-jwt-django-rest-framework-token)

```
from rest_framework_simplejwt.authentication import JWTAuthentication
JWT_authenticator = JWTAuthentication()

# authenitcate() verifies and decode the token
# if token is invalid, it raises an exception and returns 401
response = JWT_authenticator.authenticate(request)
if response is not None:
    # unpacking
    user , token = response
    print("this is decoded token claims", token.payload)
else:
    print("no token is provided in the header or the header is missing")
```

