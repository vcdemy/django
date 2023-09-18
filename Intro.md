# Django Hello World

## 官網文件連結

* [Getting started with Django](https://www.djangoproject.com/start/)

## 安裝 Django

```bash
$ pip install django
```

## 建立新 project

```bash
$ django-admin startproject mysite
```
命令執行後，會在當前目錄下建立一個mysite的目錄。

如果不想要多建立一個目錄，可以下如下指令，直接將檔案放置於當前目錄。

```bash
$ django-admin startproject mysite .
```

## 建立新 App

```bash
$ python manage.py startapp myapp
```

需將新增加的app加入到settings.py裡面的INSTALLED_APPS裡面去。

## 將server跑起來

```bash
$ python manage.py runserver
```