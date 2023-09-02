# Beginners Guide to environment variable setup (Django)
[![Author | Priyanshu Pardhi](https://www.coolgenerator.com/Data/Textdesign/202309/b4ce6200ba7cc113143116f9fc82462b.png)](https://github.com/priyanshupardhi)

**FYI**: Python virtual environment and environment variables is two different things.

We are going to talk about setting up environment variables.
Whenever we create an app there are some secret credential throughout our application that need to be secured. 
For securing an app we keep such credentials in an environment variable (Ideally we should do it before pushing and sharing to any remote resource).

## Installation
##### 1. Install Django Environ
In your terminal, inside the project directory, type:
```
$ pip install django-environ
```
##### 2. Import environ
Inside **setting.py**
```
import environ
```
##### 3. Initialise environ
Below your import in settings.py:
```
import environ
# Initialise environment variables
env = environ.Env()
environ.Env.read_env()
```
##### 4. Create your .env file
In the same directory as settings.py, create a file called **.env**, keep the essential and secured credential there, below we have added some common environment variable

```
SERVER=dev
DEBUG=True
SECRET_KEY=<project's secret key>

ALLOWED_HOSTS=['*']                # where '*' represents a wild-card to accept any HTTP host value.

# Database setting
DB_NAME=
DB_USER=
DB_PASSWORD=
DB_HOST=
DB_PORT=
```
##### 5. Replace all references to your environment variables in settings.py

since we have mentioned these environment variable here, we need a way to access this variables wherever we are using these variables, for that we need to replace all references to your environment variables in settings.py
```
...
import os
from pathlib import Path
import environ
.
.
env = environ.Env(DEBUG=bool, ALLOWED_HOSTS=list)
env_file = BASE_DIR / '.env'               # path to your .env file

environ.Env.read_env(env_file=env_file, overwrite=True)
...
SECRET_KEY = env('SECRET_KEY')
...
DEBUG = env('DEBUG')
...
ALLOWED_HOSTS = env('ALLOWED_HOSTS')
...
if DEBUG:
    DATABASES = {
        'default': {
            'ENGINE': 'django.db.backends.sqlite3',
            'NAME': BASE_DIR / 'db.sqlite3',
        }
    }
else:
    DATABASES = {
        'default': {
            'ENGINE': 'django.db.backends.postgresql_psycopg2',
            'NAME': env('DB_NAME'),
            'USER': env('DB_USER'),
            'PASSWORD': env('DB_PASSWORD'),
            'HOST': env('DB_HOST'),
            'PORT': env('DB_PORT'),
        }
    }
```

> IMPORTANT: Add your .env file to `.gitignore`

Now you are ready to push your code 🚀.

## Reference
[How to set up environment variables in Django](https://alicecampkin.medium.com/how-to-set-up-environment-variables-in-django-f3c4db78c55f)

**Feel free to share! 👍**
