## Link
[Celery](https://knowpapa.com/django-celery-rabbitmq/)
## Install

```
sudo pip3 install celery==4.2.1
sudo pip3 install redis==3.2.0
```

## Config
### 1. add to `settings.py`

```
CHANNEL_LAYERS = {
    'default': {
        'BACKEND': 'channels_redis.core.RedisChannelLayer',
        'CONFIG': {
            "hosts": [('127.0.0.1', 6379)],
        },
    },
}
CELERY_BROKER_URL = 'redis://localhost'
CELERY_RESULT_BACKEND = 'redis://localhost'
```

### 2. Next, create a `__init__.py` file in your Project root directory and add the following code to it:

[__init__.py](https://github.com/vuvandang1995/2019_VVD/blob/master/Django/config/__init__.py)

### 3. Create `celery.py` file in same folder with `settings.py`

[celery.py](https://github.com/vuvandang1995/2019_VVD/blob/master/Django/config/celery.py)

- Change content in 4th and 6th lines to your-project-name

### 4. Build Celery tasks

- example:
  - Create `tasks.py` in your app
  
  [tasks.py](https://github.com/vuvandang1995/2019_VVD/blob/master/Django/config/tasks.py)
  
### Use Celery tasks

```
mail_subject = 'Active account OpenID'
message = render_to_string('client/active_acc.html', {
    'user': user,
    'domain': current_site.domain,
    'uid': urlsafe_base64_encode(force_bytes(user.id)).decode(),
    'token': account_activation_token.make_token(user)
})
send_email.delay(mail_subject, message, user.email)
```

## Run Celery
### With command

`celery worker -A openid worker -l info`

### With systemd
- Create celery environment file
    
    `vim /home/openid/OpenID_MDT/openid/celery_env_file`
    
- [celery_env_file](https://github.com/vuvandang1995/2019_VVD/blob/master/Django/config/celery_env_file)
    
    - Create folder
    
    `sudo mkdir /var/run/celery /var/log/celery`
    
- Create service
    
    `sudo vim /etc/systemd/system/celery.service`
    
- [celery.service](https://github.com/vuvandang1995/2019_VVD/blob/master/Django/config/celery.service)
    
- **Note: Change the files above to suit you**

