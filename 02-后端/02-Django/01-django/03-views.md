# 1. 视图

## 1.1 视图函数

### 1.1.1 创建的视图

手动创建视图函数, 和flask模式差不多

```python
from django.http import HttpResponse
def index(request, *args, **kwargs):
    """主页"""
    if request.method == "GET":
        return HttpResponse("Get")
    elif request.method == "POST":
        return HttpResponse("Post")
    return HttpResponse("Method Not Allowed {}: {}".format(request.method, request.path), status=405)

```

### 1.1.2 路由配置

```python
urlpatterns = [
    url(r"/", index),
]
```



## 1.2 视图类

### 1.2.1 自动分发

视图类可以自动完成method的分发, 相关源码如下:

```python
	def as_view(cls, **initkwargs):
        """Main entry point for a request-response process."""
        ...

        def view(request, *args, **kwargs):
            ...
            return self.dispatch(request, *args, **kwargs)
			...
        return view
    
	def dispatch(self, request, *args, **kwargs):  # 动态分发请求
        # Try to dispatch to the right method; if a method doesn't exist,
        # defer to the error handler. Also defer to the error handler if the
        # request method isn't on the approved list.
        if request.method.lower() in self.http_method_names:
            handler = getattr(self, request.method.lower(), self.http_method_not_allowed)
        else:
            handler = self.http_method_not_allowed
        return handler(request, *args, **kwargs)

    def http_method_not_allowed(self, request, *args, **kwargs):  # 不支持请求
        logger.warning(
            'Method Not Allowed (%s): %s', request.method, request.path,
            extra={'status_code': 405, 'request': request}
        )
        return HttpResponseNotAllowed(self._allowed_methods())
```

* 从源码中可以看出, dispatch函数为主要的分发函数, 配置在路由中必须是这个函数, 所以配置路由时候一定要先执行as_view,  才能获取到dispath.

### 1.2.2 视图类定义

```python
class RequestTest(View):
    """不同的请求使用不同的方法"""

    def get(self, request, *args, **kwargs):
        return HttpResponse("get")

    def post(self, request, *args, **kwargs):
        print(request.GET)
        return HttpResponse("post")
```

### 1.2.3 路由配置

```python
urlpatterns = [
    url(r"/", RequestTest.as_view(), name="test"),
]
```

# 2. MiXin

