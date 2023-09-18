# Views

Django的Views有兩種。這裡講的是Function Views，另外一種是 Class-Based Views (CBVs)，CBVs會在另外一個.md檔中介紹。

## 官網文件連結

* [Writing views](https://docs.djangoproject.com/en/4.2/topics/http/views/)

## 簡單範例

```python
# views.py
from django.http import HttpResponse
import datetime


def current_datetime(request):
    now = datetime.datetime.now()
    html = "<html><body>It is now %s.</body></html>" % now
    return HttpResponse(html)
```

上面是 view 的簡單範例，跟他對應的 urls.py 可能是像這樣：

```python
# urls.py
from django.urls import path

from . import views

urlpatterns = [
    path("current_datetime/", views.current_datetime)
]
```

