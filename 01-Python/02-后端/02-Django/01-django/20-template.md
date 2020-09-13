# 1. django模板

## 1.1 模板语言

## 1.2 模板配置

```python
TEMPLATES = [
    {
        "DIRS": [os.path.join(BASE_DIR, "template")],
    }
]
```

## 1.3 视图渲染

### 1.3.1 render

```Python
from django.shortcuts import render

def index(request):
    data = {"city": "上海"}
    return render(request, "index.html", data)
```

### 1.3.2 loader

```python
from django.http import HttpResponse
from django.template import loader

def index(request):
    # 获取模板
    template = loader.get_template("index.html")
    # 准备数据
    data = {"city": "北京"}
    # 模板渲染
    return HttpResponse(template.render(data))
```



# 2. 模板语言

## 2.1 基本语法

### 2.1.1 循环

支持list, tuple, dict等可迭代对象的遍历

```jinja2
{% for book in books %}
	{{ forloop.counter }}  # 获取当前循环次数, 从1开始
	{{ book.descript }}  # 调用属性
	{{ book.content.1 }}  # 列表索引取值
{% empty %}  # 列表为空或不存在时执行此逻辑
	{{ do something }}
{% endfor %}
```

### 2.1.2 判断

```jinja2
{% if xx %}
	{{ do something1 }}
{% elif xx %}
	{{ do something2 }}
{% else %}
	{{ do something3 }}
{% endif %}
```

### 2.1.3 运算符

```jinja2
{% if a==1 %}  # "=="两边不得有空格
```

条件判断连接符: `==`, `!=`, `>`, `>=`, `<=`, `and`, `or`, `not`

## 2.2 过滤器

| 参数      | 说明                                      |
| --------- | ----------------------------------------- |
| `safe`    | 禁用转义                                  |
| `length`  | 长度,  `{{ xxx | length: 2 }}`            |
| `default` | 默认值, `{{ data | default: "value" }}`   |
| `date`    | 时间, `{{ value | date: "Y-m-d H:i:s" }}` |
|           |                                           |
|           |                                           |
|           |                                           |
|           |                                           |
|           |                                           |

## 2.3 注释

* 单行注释

  ```jinja2
  {# xxx #}
  ```

* 多行注释

  ```jinja2
  {% comment %}
  	xxx
  {% endcomment %}
  ```

## 2.4 模板继承

* 父类中定义

  ```jinja2
  {% block name %}
  	do something1
  {% endblock name %}
  
  {% block f_block %}
  	do something2
  {% endblock f_block %}
  ```

  

* 子类中调用

  ```jinja2
  {% extends "xx.html" %}
  
  {% block name %}
  	需要修改的内容
  {% endblock name %}
  
  # 可以继承父类的block
  {% block name %}
  	{{ f_block.super }}
  {% endblock name %}
  ```

  

