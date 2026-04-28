## pip install python-dotenv
## pip install dj-database-url psycopg2-binary

>> in settings.py add this where alll imports are there.
    from dotenv import load_dotenv
    import dj_database_url

    BASE_DIR = path...

    load_dotenv()

>> And in databases at settings.py change it with this.
    DATABASES = {
        'default': dj_database_url.parse(
            os.getenv("DATABASE_URL"),
            conn_max_age=600
        )
    }

## pip install python-dotenv

>> create .env as root.
    DATABASE_URL=...

>> This command will add all the tables in the database.
## python manage.py migrate

>> To test the connection.
## python manage.py shell
>> In shell write this two lines.
    from django.contrib.auth.models import User
    User.objects.count()

>> Now lets create the superuser.
## python manage.py createsuperuser