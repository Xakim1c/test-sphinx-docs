Installation
============

Система приложений состоит из 3 компонент:

1. **Giga Turnip** - это Django приложение для обслуживания backend логики
2. **Giga Turnip Front End** - это React приложение для создания произвольных Кампаний
3. **Giga Turnip Task View** - это React приложение (клиент для пользователей) в котором пользователи могут выполнять свои задания

*прежде чем начать развертывание приложений, необходимо создать новый проект в Google Cloud Platform* **(GCP)** *и подключить его к* **Firebase** *(как это сделать - смотрите ниже в инструкции, удачи!)*

1. Google Cloud Platform
------------------------
Создайте новый проект в GCP_ (например: Jokes Collector)

.. _GCP: https://console.cloud.google.com/

.. image:: _static/gcp_create.png


2. Firebase
-----------
В консоле Firebase_ подключите Ваш созданный проект из **GCP**

.. _Firebase: https://console.firebase.google.com/

.. image:: _static/firebase_create.png

Перейдите на вкладку **Authentication** и включите sign-in через **Email** и **Google**

.. image:: _static/firebase_google_auth.png

3. Credentials
--------------
Для связи приложений с Firebase Вам необходимо получить **Firebase configuration** + создать **json ключ** через GCP:

1. Перейдите на свой проект в **Firebase** на закладку **Settings**
2. На закладке **General** добавьте *Web App* (красная стрелочка)

.. image:: _static/firebase_web_app.png
.. image:: _static/firebase_web_app_create.png

3. После создания обратие внимание на переменную **firebaseConfig**, эти данные нам понадобятся немного позже

.. image:: _static/firebase_config.png

4. Для создания **firebase json key** перейдите на свой проект в **GCP** на закладку **IAM & Admin -> Service Accounts** (для перехода в настройки сервисного аккаунта firebase - нажмите на него)

.. image:: _static/gcp_json_key_1.png

5. Создайте новый ключ

.. image:: _static/gcp_json_key_2.png

6. Выберите тип JSON, после подтверждения на Ваш компьютер будет скачан json файл, будем называть его с Вами **firebase json key** перенесите этот ключ в любую папку по Вашему усмотрению. Он нам понадобится немного позже

.. image:: _static/gcp_json_key_3.png

Отлично! Мы закончили с настройкой **GCP** + **Firebase**. Теперь у нас имеются: **firebaseConfig** и **firebase json key**. Самое время начать локально развертывать наши приложения!

4. Giga Turnip
---------------
**Giga Turnip** - это *Django* приложение для обслуживания backend логики

First, obtain Python_ and pipenv_ if you do not already have them. Using a
virtual environment will make the installation easier, and will help to avoid
clutter in your system-wide libraries. You will also need Git_ in order to
clone the repository.

.. _Python: http://www.python.org/
.. _pipenv: https://pipenv.pypa.io/en/latest/
.. _Git: http://git-scm.com/

Создайте папку где будет развернуты все ваши приложения проекта, например *JokesCollector*::

    mkdir JokesCollector
    cd JokesCollector


Клонируем репозиторий::

    git clone https://github.com/KloopMedia/GigaTurnip.git
    cd GigaTurnip

Устанавливаем зависимости::

    pipenv install
    pipenv shell

Делаем миграцию::

    python manage.py makemigrations
    python manage.py migrate

Создаем суперюзера::

    python manage.py createsuperuser

.. note::
    set ``FIREBASE_SERVICE_ACCOUNT_KEY`` environment variable

    для этого установите значение FIREBASE_SERVICE_ACCOUNT_KEY значением содержимого файла **firebase json key** (если забыл где это смотри **3. Credentials**)

Example::

    export FIREBASE_SERVICE_ACCOUNT_KEY='{
          "type": "service_account",
          "project_id": "jokes-collector",
          "private_key_id": "your_private_key_id",
          "private_key": "your_key",
          "client_email": "your_client_email",
          "client_id": "your_client_id",
          "auth_uri": "your_auth_uri",
          "token_uri": "your_token_uri",
          "auth_provider_x509_cert_url": "your_auth_provider_x509_cert_url",
          "client_x509_cert_url": "your_client_x509_cert_url"
    }'


.. note::
    In ``settings.py`` add 'http://localhost:3000' and 'http://localhost:3001' into ``CORS_ORIGIN_WHITELIST`` variable

Example::

    CORS_ORIGIN_WHITELIST = [
        'http://localhost:3000',
        'http://localhost:3001'
    ]

To actually get Giga Turnip running, do the following::

    python manage.py runserver


This will give you a locally running instance http://127.0.0.1:8000/

.. image:: _static/django_admin.png

Отлично! Мы закончили установку **Giga Turnip**! Переходим к установке **Giga Turnip Front End**

5. Giga Turnip Front End
------------------------
**Giga Turnip Front End** - это *React* приложение для создания Кампаний (цепочек задач, а также логики их выполнения)

Клонируем репозиторий, переходим на ветку staging, устанавливаем необходимые библиотеки::

    git clone https://github.com/KloopMedia/gigaturnip-frontend.git
    cd gigaturnip-frontend
    git checkout staging
    npm install


.. note::
    Change ``firebaseConfig`` in /src/util/Firebase.js with your **firebaseConfig** (если забыл где это смотри **3. Credentials**)

Example::

    const firebaseConfig = {
        apiKey: "AIzaSyCCeI1gW0WT_PBZ6rrr2xDic15VTbge-GA",
        authDomain: "jokes-collector.firebaseapp.com",
        projectId: "jokes-collector",
        storageBucket: "jokes-collector.appspot.com",
        messagingSenderId: "500369573812",
        appId: "1:500369573812:web:29d2abca2ff8f93111d4e1"
      };

Чтобы запустить приложение::

    npm start

This will give you a locally running instance http://127.0.0.1:3000/

.. image:: _static/frontend_run.png

Поздравляем! Вы на верном пути! Осталось установить последнее приложение!


6. Giga Turnip Task View
------------------------
**Giga Turnip Task View** - это React приложение (клиент для пользователей) в котором пользователи могут выполнять свои задания::

    git clone https://github.com/KloopMedia/GigaTurnipTaskView.git
    cd GigaTurnipTaskView
    npm install

.. note::
    Change ``firebaseConfig`` in /src/util/Firebase.js with your **firebaseConfig** (если забыл где это смотри **3. Credentials**)

Example::

    const firebaseConfig = {
        apiKey: "AIzaSyCCeI1gW0WT_PBZ6rrr2xDic15VTbge-GA",
        authDomain: "jokes-collector.firebaseapp.com",
        projectId: "jokes-collector",
        storageBucket: "jokes-collector.appspot.com",
        messagingSenderId: "500369573812",
        appId: "1:500369573812:web:29d2abca2ff8f93111d4e1"
      };

Чтобы запустить приложение::

    npm start


This will give you a locally running instance http://127.0.0.1:3001/

.. image:: _static/frontend_task_view_run.png

Ура! Мы закончили с установкой! Можем переходить к настройке приложений!