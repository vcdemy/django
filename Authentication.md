# User Authentication

## 官網文件連結
* https://docs.djangoproject.com/en/4.2/topics/auth/
* https://docs.djangoproject.com/en/4.2/topics/auth/default/

## 準備動作

通常在project建立後，User Authentication需要的App等東西，應該都已經自動被加入 settings.py。

接著，我們要執行 migrate 來建立資料庫內的資料表，再執行 createsuperuser 來建立後台的管理帳號。

```bash
$ python manage.py migrate
```

```bash
$ python manage.py createsuperuser
```

createsuperuser時，也可以加參數，直接把資料填上去，如：
```bash
$ python manage.py createsuperuser --username=joe --email=joe@example.com
```

## Project的URLconf設定

使用者登錄相關的路徑，可以使用底下的設定。

```python
# project的urls.py中
urlpatterns = [
    path("accounts/", include("django.contrib.auth.urls")),
]
```

上面的設定，其實就把底下的路徑都設好了：

```
accounts/login/ [name='login']
accounts/logout/ [name='logout']
accounts/password_change/ [name='password_change']
accounts/password_change/done/ [name='password_change_done']
accounts/password_reset/ [name='password_reset']
accounts/password_reset/done/ [name='password_reset_done']
accounts/reset/<uidb64>/<token>/ [name='password_reset_confirm']
accounts/reset/done/ [name='password_reset_complete']
```

預設對應的template，都需要放在<font color="red">/registration/</font>底下。如：
"/templates/registration/login.html"

不然，就要使用類似如下的設定去修改template的位置。

(這個意思是說，所有的路徑都要自己一個一個加到URLconf裡面去。)

```python
urlpatterns = [
    path(
        "change-password/",
        auth_views.PasswordChangeView.as_view(template_name="change-password.html"),
    ),
]
```

路徑設定好之後，在template中，可以使用 form 這個變數建立表單。

```html
<form action="" method="post">
    {% csrf_token %}
    {{ form }}
    <input type="submit" value="submit">
</form>
```

## 存取會員限定的網頁

如果要存取會員限定的網頁，大致上可以使用兩種方法：
* login_required的decorator (Function Views)
* LoginRequiredMixin (Class-Based Views)

### @login_required範例
```python
from django.contrib.auth.decorators import login_required


@login_required(login_url="/accounts/login/")
def my_view(request):
    ...
```

### LoginRequiredMixin範例
```python
from django.contrib.auth.mixins import LoginRequiredMixin


class MyView(LoginRequiredMixin, View):
    login_url = "/login/"
    redirect_field_name = "redirect_to"
```
