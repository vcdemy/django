# Django Class-Based Views

## 官網文件連結

* https://docs.djangoproject.com/en/4.2/topics/class-based-views/


## RedirectView

如果在 project 的根目錄裡面，沒有設定 templates，可以直接使用 RedirectView 把網頁根目錄的路徑設到 RedirectView 重新定向的目標。

### 快速做法

```python
RedirectView.as_view(url='/URL/')
```

```python
from django.urls import path
from django.views.generic.base import RedirectView

urlpatterns = [
    path('redirect/', RedirectView.as_view(url='/new-url/'), name='my-redirect'),
]
```

### 一般做法

```python
# some_app/views.py
from django.views.generic.base import RedirectView

# 内部 URL 重定向範例
class MyRedirectView(RedirectView):
    url = '/new-url/'  # 要重定向到的目標 URL

# 外部 URL 重定向範例
class ExternalRedirectView(RedirectView):
    url = 'https://www.example.com/'  # 要重定向到的外部 URL
    permanent = False  # 設定為 True 表示永久重定向（HTTP 301），預設為 False（HTTP 302）
```

```python
# urls.py
from django.urls import path
from .views import MyRedirectView, ExternalRedirectView

urlpatterns = [
    path('redirect/', MyRedirectView.as_view(), name='my-redirect'),
    path('external/', ExternalRedirectView.as_view(), name='external-redirect'),
]
```
## TemplateView

Reference:

* https://docs.djangoproject.com/en/4.2/topics/class-based-views/

### 快速做法

```python
# urls.py
from django.urls import path
from django.views.generic import TemplateView

urlpatterns = [
    path("about/", TemplateView.as_view(template_name="about.html")),
]
```

### 一般做法

```python
# some_app/views.py
from django.views.generic import TemplateView


class AboutView(TemplateView):
    template_name = "some_app/about.html"
```

```python
# urls.py
from django.urls import path
from some_app.views import AboutView

urlpatterns = [
    path("about/", AboutView.as_view()),
]
```

## FormView

Reference: 

* https://docs.djangoproject.com/en/4.2/topics/class-based-views/generic-editing/


需設定屬性：

* form_class: Form的類別名稱
* template_name: Template名稱
* success_url: 成功之後要連到哪裡
* form_valid(self, form): 成功之後，怎麼處理資料


```python
# forms.py

from django import forms


class ContactForm(forms.Form):
    name = forms.CharField()
    message = forms.CharField(widget=forms.Textarea)

    def send_email(self):
        # send email using the self.cleaned_data dictionary
        pass
```

```python
# views.py
from myapp.forms import ContactForm
from django.views.generic.edit import FormView


class ContactFormView(FormView):
    template_name = "contact.html"
    form_class = ContactForm
    success_url = "/thanks/"

    def form_valid(self, form):
        # This method is called when valid form data has been POSTed.
        # It should return an HttpResponse.
        form.send_email()
        return super().form_valid(form)
```

## ListView, DetailView, CreateView, UpdateView, DeleteView

### ListView

Reference:

* https://docs.djangoproject.com/en/4.2/topics/class-based-views/generic-display/

類別的屬性名稱：
* model
* queryset: 設定要傳給template的資料。
* context_object_name: 預設在template中使用object_list，可以使用這個屬性改變名稱。
* template_name: 設定template的名稱，如果沒有設定，預設使用'`model`-list.html'。


### DetailView

Reference:

* https://docs.djangoproject.com/en/4.2/intro/tutorial04/

類別的屬性名稱：

* model

在URLconf裡面設定需要傳入pk (primary key)。

### CreateView

類別的屬性名稱：
* model
* fields
* success_url
* template_name_suffix: 預設是"_form"，所以template的名字就是"`model`_form.html"

當按了submit，資料就會自動進資料庫。

### UpdateView

UpdateView可以使用跟CreateView一樣的template即可。

類別的屬性名稱：
* model
* fields
* success_url
* template_name_suffix: 預設是"_form"，所以template的名字就是"`model`_form.html"

跟CreateView主要的差別，應該是在URLconf設定時，需要設定pk。

### DeleteView

Reference:

* https://docs.djangoproject.com/en/4.2/ref/class-based-views/generic-editing/

```python
# myapp/views.py
from django.urls import reverse_lazy
from django.views.generic.edit import DeleteView
from myapp.models import Author


class AuthorDeleteView(DeleteView):
    model = Author
    success_url = reverse_lazy("author-list")
```

```python
# myapp/auther_confirm_delete.html
<form method="post">{% csrf_token %}
    <p>Are you sure you want to delete "{{ object }}"?</p>
    {{ form }}
    <input type="submit" value="Confirm">
</form>
```