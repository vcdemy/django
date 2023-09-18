# Static Files

## 官網文件連結

* [How to manage static files (e.g. images, JavaScript, CSS)](https://docs.djangoproject.com/en/4.2/howto/static-files/)
* [How to deploy static files](https://docs.djangoproject.com/en/4.2/howto/static-files/deployment/)
* [Django Settings](https://docs.djangoproject.com/en/4.2/ref/settings/)

## 概略說明

### settings.py中的設定

在settings.py裡面的INSTALLED_APPS中，需要有django.contrib.staticfiles (預設就有加入了)。

另外，也需設定STATIC_URL (如下，預設已經有設定)：

```python
STATIC_URL = "static/"
```

### template中的使用

在 tempate 中，需要使用 {% load static %} 相關資源。

然後，可以使用 {% static %} 反查出靜態資源相關的網址。

```html
{% load static %}

<img src="{% static 'my_app/example.jpg' %}" alt="My image">
```

### app相關的靜態資源

跟各個app相關的靜態資源通常存在，app底下的static目錄。

我們通常會在 static 目錄底下建立一個 app 名稱的目錄，再將檔案放在裡面。

如： my_app/static/my_app/example.jpg
