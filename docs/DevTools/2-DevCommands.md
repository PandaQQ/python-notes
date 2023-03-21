# 开发常用工具(Commands & Tools)

#### 1. Django 项目常用

```bash
python manage.py makemigrations

python manage.py migrate

python manage.py createsuperuser

# !!!! when table not create
python manage.py migrate --run-syncdb

# python package locked
pip3 freeze > requirements.txt

# pyhotn install from requirements
pip3 install -r requirements.txt
```



#### 2. Docker Python Environment

```bash
docker run -it --name pytest \
-v /Users/qzheng/PycharmProjects/medium:/root/medium \
python:3.11.1-slim-bullseye bash

# 保存鏡像
docker commit -a "pandaqq" -m "my python env" 80cdd11f9b60  hello:v1
```



#### 3. Postman 配置

> 因为大多数接口需要使用认证才可以调用，因此需要对postman进行一些必要的配置。
>
> 1. 创建 Collections 时，可以指定 Authorization ，类型（Type）选择 "Bear Token", Token 的值配置为 "{{token}}"，意思是从postman环境变量中获取token的值。
>2. 配置 Pre-request Script，脚本内容为:

```
const getTokenByPostRequest = {
    url: 'http://127.0.0.1:8769/api/oauth/login/',
    method: 'POST',
    header: 'Content-Type: application/json',
    body: {
        mode: 'raw',
        raw: JSON.stringify({"username": "admin", "password": "123456"}),
    }
};
pm.sendRequest(getTokenByPostRequest, function (err, response) {
        pm.environment.set("token", response.json().data.token);
    }
);
```

> 上述代码中的用户名和密码如果已经更改，则需要改为自己定义的用户名和密码。



#### 4. Debug in Django

>  先创建一个 "Run/Debug Configuration" ，然后即可使用 PyCharm 自带的 debug 功能，方便图形化操作了。
>
>  **debug variables of anywhere in django project**

参见 issue: [#11](https://github.com/TianPangJi/drf_admin/issues/11)

```python
import logging

logger = logging.getLogger('django.request')

# do this anywhere
logger.info(" ========> " + " ".join([__name__, str(request), str(kwargs)]))
```



#### 5.  drf-yasg(Swagger升级版)

##### 5.1drf-yasg介绍

参考[drf-yasg官网](https://drf-yasg.readthedocs.io/)，`drf-yasg`是基于`Swagger`和`OpenAPI 2.0`规范的`API`文档自动化生成工具，能够生成比原生`swagger`更为友好的`API`文档界面

目前的兼容性如下

- **Django Rest Framework**: 3.10, 3.11, 3.12
- **Django**: 2.2, 3.0, 3.1
- **Python**: 3.6, 3.7, 3.8, 3.9



##### 5.2 安装drf-yasg库

**在操作下面的步骤前请将第3节`swagger`相关内容全部注释或还原**

```javascript
pip3 install drf-yasg
pip3 freeze > requirements.txt
```



##### 5.3 配置app

```javascript
INSTALLED_APPS = [
    ...
    'rest_framework',
    'drf_yasg'
]
```



##### 5.4 配置路由url

```javascript
from rest_framework import permissions
from drf_yasg.views import get_schema_view
from drf_yasg import openapi
...

schema_view = get_schema_view(
    # 具体定义详见 [Swagger/OpenAPI 规范](https://swagger.io/specification/#infoObject)
    openapi.Info(
        title="Snippets API",
        default_version='v1',
        description="Test description",
        terms_of_service="https://www.google.com/policies/terms/",
        contact=openapi.Contact(email="contact@snippets.local"),
        license=openapi.License(name="BSD License"),
    ),
    # public 表示文档完全公开, 无需针对用户鉴权
    public=True,
    # 可以传递 drf 的 BasePermission
    permission_classes=(permissions.AllowAny,),
)

urlpatterns = [
    # drf_yasg
    re_path(r'^swagger(?P<format>\.json|\.yaml)$', schema_view.without_ui(cache_timeout=0), name='schema-spec'),
    re_path(r'^swagger/$', schema_view.with_ui('swagger', cache_timeout=0), name='schema-swagger-ui'),
    re_path(r'^redoc/$', schema_view.with_ui('redoc', cache_timeout=0), name='schema-redoc'),
    ...
]
```

复制

`drf-yasg`会暴露`4`种默认路径`endpoint`, 分别为:

- `/swagger.json`, JSON 格式的 API 定义
- `/swagger.yaml`, YAML 格式的 API 定义
- `/swagger/`, 基于原生 swagger-ui 样式的前端页面
- `/redoc/`, 基于 ReDoc 样式的前端页面



##### 5.5 访问查看

完成后重启项目进行访问 ~ 



##### 4.6 更多配置及说明



##### 4.6.1 get_schema_view的配置

函数 **get_schema_view** 的作用是返回自动生成 API 文档的视图类, 该函数接受以下参数:

- **info**: `Swagger API Info`对象, 具体定义详见 [Swagger/OpenAPI 规范](https://swagger.io/specification/#infoObject), 如果缺省, `drf-yasg`默认会用 `DEFAULT_INFO` 进行填充
- **url**: 项目API的基础地址, 如果缺省, 则根据视图所在的位置进行推导
- **patterns**: 自定义的`urlpatterns`, 该参数直接透传至`SchemaGenerator`
- **urlconf**: 描述从哪个文件获取路由配置, 缺省值是`urls`, 该参数直接透传至`SchemaGenerator`
- **public**: 描述API文档是否公开, 如果未 `False`, 则仅返回当前用户具有权限的接口`endpoints`的`API`文档
- **validators**: 用于校验自动生成的`Schema`的校验器, 目前仅支持 `ssv` 和 `flex`
- **generator_class**: 自定义`OpenAPI schema`生成器类, 该类应该继承自 `OpenAPISchemaGenerator`
- **authentication_classes**: 用于`schema view`进行登录认证的类
- **permission_classes**: 用于`schema view`进行权限校验的类



##### 4.6.2 SchemaView 的配置

通过函数`get_schema_view`可以获取对应的`SchemaView`, 调用该类的`with_ui`或 `without_ui`方法可生成对应的`视图函数`, 将其添加进`urlpatterns`即可访问到自动生成的 API 文档

- **SchemaView.with_ui(renderer, cache_timeout, cache_kwargs)**: 返回使用指定`UI`渲染器的视图函数, 可选的`UI`渲染器有: `swagger`, `redoc`。
- **SchemaView.without_ui(cache_timeout, cache_kwargs)**: 返回无`UI`的视图函数, 该函数可以返回`json/yaml`格式的`swagger`文档

以上两个函数均支持通过 `cache_timeout` 或 `cache_kwargs` 配置缓存参数



##### 4.6.3 缓存的配置

由于`schema`通常在服务运行期间不会发生改变, 因此 `drf-yasg`使用`django`内置的 `cache_page` 实现开箱即用的缓存功能, 只需要配置对应的参数即可启用, 对应参数解释如下:

- **cache_timeout**: 用于指定缓存的生存时间
- **cache_kwargs**: 用于传递 **cache_page** 允许接受的非位置参数, 如 `cache`(指定 cache backend), `key_prefix`(缓存`key`的前缀) 等等, 详见`django`官方文档

>  需要注意的是, 由于 **drf-yasg** 支持针对不同用户返回不一样的 API 文档(通过**public**、**authentication_classes**、**permission_classes**等参数配置), 因此对于不同用户(通过HTTP 请求头中的 **Cookie** 和 **Authorization** 进行区分), 会在内存中分别进行缓存。 



##### 4.6.4 校验文档有效性

为保证自动生成文档的有效性, 可以通过在`get_schema_view`中设置 `validators` 参数开启校验自动化生成文档是否符合`OpenAPI2.0`规范的功能



##### 4.6.5 代码自动生成

使用`Swagger/OpenAPI`规范生成文档的好处之一, 就是能通过`API`文档自动生成**不同语言**的 SDK，该功能由[swagger-codegen](https://github.com/swagger-api/swagger-codegen)提供







```
[AUTH.ldap]
host_url=auth-dc.hsu.edu.hk
user_domain=hsu.edu.hk
ldap_version=3
objectclass="(&(objectClass=user)(sAMAccountName={0})(!(userAccountControl:1.2.840.113556.1.4.803:=2)))"
contexts="OU=HS Users,DC=hsu,DC=edu,DC=hk"

```

