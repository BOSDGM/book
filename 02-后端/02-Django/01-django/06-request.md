# 1. 数据提取

## 1.1 url参数

和flask参数提取类似

```python
request.GET.get("a")     # 4 
request.GET.get("b")     # 7
request.GET.getlist("a") # ["4", "7"] 参数名字重复, 只能获取最后一个, 通过getlist可以获取重复的列表
```

## 1.2 Form表单

```python
request.POST, 不论表单提交类型, 均以POST获取
```



## 1.3 JSON数据

## 1.4 流数据

# 2. headers

## 2.1 

