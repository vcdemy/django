# Forms

## 官網文件連結

* https://docs.djangoproject.com/en/4.2/topics/forms/
* [Form and field validation](https://docs.djangoproject.com/en/4.2/ref/forms/validation/)

## HTML Form 範例

### HTTP GET 方法

```html
<form action="/my_view/" method="get">
    <label for="your_name">Your name: </label>
    <input type="text" name="your_name">
    <input type="submit" value="OK">
</form>
```

```python
def my_view(request):
    if "your_name" in request.GET and request.GET['your_name']!="":
        return HttpResponse("Hello, " + request.GET['your_name'])
    else:
        return HttpResponse("Hello, John Doe!")
```

### HTTP POST 方法

```html
<form action="/my_view/" method="post"> {% csrf_token %}
    <label for="your_name">Your name: </label>
    <input type="text" name="your_name">
    <input type="submit" value="OK">
</form>
```

```python
def my_view(request):
    if request.POST:
        if request.POST['your_name']:
            return HttpResponse("Hello, " + request.POST['your_name'])
        else:
            return HttpResponse("Hello, John Doe!")
    else:
        return HttpResponse("Hello, John Doe!")
```

## Django Forms


### Form 範例
```python
# forms.py
from django import forms


class NameForm(forms.Form):
    your_name = forms.CharField(label="Your name", max_length=100)
```

### View 範例

```python
# views.py
from django.http import HttpResponseRedirect
from django.shortcuts import render

from .forms import NameForm


def get_name(request):
    # if this is a POST request we need to process the form data
    if request.method == "POST":
        # create a form instance and populate it with data from the request:
        form = NameForm(request.POST)
        # check whether it's valid:
        if form.is_valid():
            # process the data in form.cleaned_data as required
            # ...
            # redirect to a new URL:
            return HttpResponseRedirect("/thanks/")

    # if a GET (or any other method) we'll create a blank form
    else:
        form = NameForm()

    return render(request, "name.html", {"form": form})
```

### Template 範例
```html
<!-- name.html -->
<form action="/your-name/" method="post">
    {% csrf_token %}
    {{ form }}
    <input type="submit" value="Submit">
</form>
```

上述的{{ form }}，也可以拆成像底下的方式，比較容易apply css style。

```html
{% for field in form %}
    <div class="fieldWrapper">
        {{ field.errors }}
        {{ field.label_tag }} {{ field }}
    </div>
{% endfor %}
```