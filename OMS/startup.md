# NOTE

LOGIN_REDIRECT_URL = '/accounts/profile/'

第一步实现：指定用户表 访问FBA_Recall 用户表

```
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.sqlite3',
        'NAME': os.path.join(BASE_DIR, 'db.sqlite3'),
    },
    'auth_db': {
        'NAME': 'FBA_Recall',
        'ENGINE': 'django.db.backends.mysql',
        'USER': 'root',
        'HOST': '127.0.0.1',
        'PASSWORD': '',
    },
}
DATABASE_ROUTERS = ['main_entry.database_routes.routers.AuthRouter']
```

第二步实现：