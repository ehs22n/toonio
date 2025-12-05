**toonio** 是一个用于以 **Toon** 格式发送数据的包，`其速度比 JSON 快数倍`。

该库还支持与 **Django** 视图集成，并提供与 **FastAPI** 一起使用的兼容性。

该库为 **Django** 提供了两个重要功能。例如，你可以创建 `动态` 视图，根据请求返回 `JSON` 或 `Toon`。

# toonio — Django 集成指南

## 使用方式

### 为所有请求返回 **Toon**

```python
REST_FRAMEWORK = {
    'DEFAULT_RENDERER_CLASSES': [
        'toonio.django.render.ToonRenderer',
    ],
    'DEFAULT_PARSER_CLASSES': [
        'toonio.django.parser.ToonParser',
    ],
}
```

## 启用动态模式（基于请求头返回 JSON 或 Toon）

```python
REST_FRAMEWORK = {
    'DEFAULT_RENDERER_CLASSES': [
         'toonio.django.render.DynamicRenderer',
    ],
    'DEFAULT_PARSER_CLASSES': [
        'toonio.django.parser.DynamicParser',
    ],
}
```

# 请求头示例

**发送 JSON → 接收 Toon**:
```bash
Content-Type: application/json
Accept: application/x-toon
```

**发送 Toon → 接收 Toon**:

```bash
Content-Type: application/x-toon
Accept: application/x-toon
```

**发送 Toon → 接收 JSON**:

```bash
Content-Type: application/x-toon
Accept: application/json
```

---

## 单视图配置

### 动态视图

```python
from toonio.django.parser import DynamicParser
from toonio.django.render import DynamicRenderer

class Index(APIView):
    parser_classes = [DynamicParser]
    renderer_classes = [DynamicRenderer]

    def get(self, request, *args, **kwargs):
        return Response({"message": "Hello Toon"})
```

### 仅 Toon 视图

```python
from toonio.django.parser import ToonParser
from toonio.django.render import ToonRenderer

class Index(APIView):
    parser_classes = [ToonParser]
    renderer_classes = [ToonRenderer]

    def get(self, request, *args, **kwargs):
        return Response({"message": "Hello Toon"})
```

---

# 在 FastAPI 中使用

```python
from fastapi import FastAPI
from toonio.response import Response
from toonio.status import HTTP_200_OK

app = FastAPI()

@app.get("/")
def hello_toon():
    return Response({"Hello": "Toon"}, status_code=HTTP_200_OK)  # 返回 Toon 响应
```

---

# 贡献指南

我们热烈欢迎所有对 Toonio 的贡献！  
无论你有改进想法、发现 bug，或想添加新功能，我们都非常乐意与你合作。

### 如何贡献

- 对于小修改，欢迎直接提交 Pull Request。  
- 对于大型功能或重大变更，请先创建 Issue 讨论设计方向。  
- 请确保代码简洁、易读，并附带必要的测试。

🐞 **问题反馈**

如遇问题：

1. 请先检查是否已有相关 Issue。  
2. 若没有，请创建新的 Issue，并提供：
   - 问题的清晰描述  
   - 复现步骤  
   - Python / 框架 / 操作系统版本  
   - 相关日志或错误信息  

我们会尽力尽快响应并修复问题。

---

# Toonio 许可证（基于 MIT 的开放许可证）

Copyright (c) 2025 Toonio Developers

特此免费授予任何获得本软件及相关文档文件（“软件”）副本的人使用本软件的权限，  
无任何限制，包括但不限于：使用、复制、修改、合并、发布、分发、再许可、和/或出售软件的副本，  
并允许软件提供者这样做，但须符合以下条件：

以上版权声明和本许可声明应包含在本软件的所有副本或主要部分中。

本软件按“原样”提供，不提供任何形式的担保，无论明示或暗示，  
包括但不限于适销性、特定用途适用性及不侵权的担保。  
在任何情况下，作者或版权持有人均不对因本软件或使用本软件引起或与之相关的任何索赔、  
损害或其他责任负责，无论是在合同诉讼、侵权行为或其他情况下。

本项目完全免费且开源。  
**无任何费用、无许可费、无订阅要求**，  
你可以自由将其用于个人、教育或商业项目。