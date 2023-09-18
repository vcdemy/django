# Models

## 官網文件連結
* [Models](https://docs.djangoproject.com/en/4.2/topics/db/models/)
* [Model field reference](https://docs.djangoproject.com/en/4.2/ref/models/fields/)

## 概略說明：

### Model的簡單範例

```python
# models.py
from django.db import models


class Person(models.Model):
    first_name = models.CharField(max_length=30)
    last_name = models.CharField(max_length=30)
```

### 透過Model建立資料庫表單

先確認 app 有沒有被加入 settings.py 裡面的 INSTALLED_APPS。

使用下面指令計算須執行的 SQL 指令。

```bash
$ python manage.py makemigrations
```

使用底下指令進行資料庫操作(如建立實際的表格)。
```bash
$ python manage.py migrate
```

### 使用Django Admin後台，進行資料操作。

要登錄後台，需要先有帳號。

```bash
$ python manage.py createsuperuser
```

需要在 admin.py 中註冊 Model。

```python
# admin.py
# admin.py
from django.contrib import admin
from myapp.models import Author

admin.site.register(Author)
```

做完設定後，啟動server，再利用 superuser 的帳號密碼登錄。

```bash
$ python manage.py runserver
```

連線到：http://127.0.0.1/admin

### 使用 shell 進行資料操作

執行底下命令，載入需要的模組後，使用ORM的方式操作。

```bash
$ python manage.py shell
```