# Django Admin

## 官網文件連結

* https://docs.djangoproject.com/en/4.2/ref/contrib/admin/

## 一般用法

使用前，須建立superuser帳號！

```bash
$ python manage.py createsuperuser
```

使用一行程式，就可以把model加進後台管理。
```python
# admin.py
from django.contrib import admin
from myapp.models import Author

admin.site.register(Author)
```

## 自訂DjangoAdmin的顯示

使用 ModelAdmin。


