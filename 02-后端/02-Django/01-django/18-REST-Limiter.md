# 1. Authentication

## 1.1 认证

### 1.1.1 认证配置

```python
REST_FRAMEWORK = {
    'DEFAULT_AUTHENTICATION_CLASSES': (
        'rest_framework.authentication.BasicAuthentication',   # 基本认证
        'rest_framework.authentication.SessionAuthentication',  # session认证
    )
}
```

### 1.1.2 视图配置

```python
class ExampleView(APIView):
    permission_classes = (IsAuthenticated,)
    ...
```



## 1.2 权限

### 1.1.1 权限配置

```python
REST_FRAMEWORK = {
    'DEFAULT_PERMISSION_CLASSES': (
        'rest_framework.permissions.IsAuthenticated',  # 进行认证登录
        'rest_framework.permissions.AllowAny',  # 允许任何人
    )
}
```

### 1.1.2 视图配置

```python
class ExampleView(APIView):
    permission_classes = (IsAuthenticated,)
    ...
```







# 2. Permissions

# 3. Throttling

# 4. Filtering

# 5. Pagination
# 6. Versioning

# 7. Exceptions

# 8. api docs

