# Приложение для голосований (Polls)

## Описание проекта

Приложение для создания и прохождения опросов с системой аутентификации пользователей. 
Применение форм в Django. Аутентификация и регистрация пользователей.

---

## Установка и запуск

### Требования
- Python 3.13.3
- Django 5.2.7
- django-crispy-forms 2.4
- crispy-bootstrap5 2025.6

### Установка зависимостей

```bash
pip install -r requirements.txt
```

### Запуск приложения

```bash
python manage.py runserver
```

Приложение будет доступно по адресу: `http://127.0.0.1:8000/polls/`

---

# Структура проекта Polls

## Корневая папка mysite

```
mysite/
├── .venv/                          # Виртуальное окружение Python
├── mysite/                         # Основная папка проекта Django
│   ├── __init__.py
│   ├── asgi.py
│   ├── settings.py                 # Конфигурация Django
│   ├── urls.py                     # Главные маршруты
│   └── wsgi.py
├── polls/                          # Приложение для голосований
│   ├── migrations/                 # Миграции БД
│   ├── static/                     # Статические файлы (CSS, JS, images)
│   ├── templates/
│   │   └── polls/                  # HTML шаблоны приложения
│   │       ├── account_activation_failure.html
│   │       ├── account_activation_success.html
│   │       ├── detail.html         # Страница голосования
│   │       ├── index.html          # Главная страница со списком опросов
│   │       ├── login.html          # Форма входа
│   │       ├── poll_new_edit.html  # Форма создания/редактирования опроса
│   │       ├── register.html       # Форма регистрации
│   │       ├── results.html        # Страница результатов
│   │       └── verification_email.html # Письмо активации
│   ├── __init__.py
│   ├── admin.py                    # Конфигурация Django admin
│   ├── apps.py
│   ├── forms.py                    # Формы (QuestionForm, NewUserForm)
│   ├── models.py                   # Модели данных (Question, Choice)
│   ├── tests.py
│   ├── urls.py                     # Маршруты приложения polls
│   └── views.py                    # Представления (views)
├── templates/                      # Глобальные шаблоны
│   ├── base.html                   # Базовый шаблон со стилями
│   └── admin/                      # Кастомные шаблоны админ-панели
│       ├── base_site.html
│       └── index.html
├── .env                            # Переменные окружения (EMAIL, пароли)
├── db.sqlite3                      # База данных SQLite
├── manage.py                       # Утилита управления Django
├── requirements.txt                # Зависимости проекта
└── README.md                       # Документация
```

---

## Описание папок и файлов

### mysite/
Основная конфигурация проекта Django.

### polls/
Главное приложение с логикой для голосований.

### templates/
Глобальные HTML шаблоны, используемые всем приложением.

### .env
Хранит конфиденциальные данные (переменные окружения):
- `POLLS_EMAIL_HOST_USER` — email для отправки писем
- `POLLS_EMAIL_HOST_PASSWORD` — пароль приложения

### requirements.txt
Список зависимостей:
```
asgiref==3.10.0
crispy-bootstrap5==2025.6
Django==5.2.7
django-crispy-forms==2.4
python-dotenv==1.0.0
sqlparse==0.5.3
tzdata==2025.2
```

### db.sqlite3
SQLite база данных с таблицами:
- Users (пользователи)
- Questions (опросы)
- Choices (варианты ответов)

### manage.py
Утилита для управления Django проектом:
- `python manage.py runserver` — запуск сервера
- `python manage.py migrate` — применение миграций
- `python manage.py createsuperuser` — создание админа
- `python manage.py shell` — интерактивная оболочка
---

## Функциональность

### 1. Просмотр опросов

**Главная страница** (`/polls/`)
- Список последних 5 опубликованных опросов
- Кнопки для голосования и просмотра результатов
- Кнопка редактирования для администраторов
- Боковая панель с информацией о пользователе

### 2. Голосование

**Страница опроса** (`/polls/<id>/`)
- Отображение вопроса и вариантов ответов
- Форма для выбора одного варианта (radio button)
- Текущие результаты голосования с прогресс-барами
- Кнопка для просмотра полных результатов

**Обработка голоса** (`/polls/<id>/vote/`)
- POST запрос с выбранным вариантом
- Увеличение счетчика голосов
- Редирект на страницу результатов

### 3. Просмотр результатов

**Страница результатов** (`/polls/<id>/results/`)
- Отображение всех вариантов с количеством голосов
- Прогресс-бары с процентным соотношением
- Кнопка "Vote Again" для повторного голосования
- Кнопка редактирования для администраторов

### 4. Создание опросов

**Форма создания** (`/polls/new/`)
- **Только для администраторов (staff)**
- Поле для ввода вопроса (до 200 символов)
- Текстовая область для вариантов ответа (каждый на новой строке)
- Валидация: минимум 2 варианта ответа
- Автоматическое создание связанных объектов Choice

### 5. Редактирование опросов

**Форма редактирования** (`/polls/<id>/edit/`)
- **Только для администраторов (staff)**
- Заполнение формы текущими данными
- Удаление старых вариантов и создание новых
- Обновление времени публикации

### 6. Аутентификация

#### Регистрация (`/polls/register`)
- Форма с полями: username, email, password, password confirmation
- Валидация пароля (минимум 8 символов, не только числа)
- Создание пользователя с статусом `is_active=False`
- Отправка письма с ссылкой активации

#### Активация аккаунта
- Письмо отправляется на указанный email
- Ссылка содержит закодированный ID пользователя и токен
- Токен действует 24 часа
- Успешная активация переводит пользователя в статус `is_active=True`

#### Вход (`/polls/login`)
- Форма с полями: username, password
- Проверка учетных данных
- Создание сессии пользователя
- Редирект на страницу, откуда пришел пользователь (если указан параметр `next`)
- Вывод сообщения об успешном входе

#### Выход (`/polls/logout`)
- Удаление сессии пользователя
- Редирект на главную страницу
- Вывод сообщения об успешном выходе

---

## Модели данных

### Question
```python
question_text    # CharField (max_length=200)
pub_date         # DateTimeField
```

Методы:
- `__str__()` - возвращает текст вопроса
- `was_published_recently()` - проверяет, опубликован ли вопрос в последние 24 часа

### Choice
```python
question    # ForeignKey to Question (CASCADE)
choice_text # CharField (max_length=200)
votes       # IntegerField (default=0)
```

Методы:
- `__str__()` - возвращает текст варианта ответа

---

## Формы

### QuestionForm (ModelForm)
- **Поля:**
  - `question_text` - текст вопроса
  - `choices` - варианты ответов (текстовая область)

- **Валидация:**
  - Минимум 2 варианта ответа
  - Очистка пустых строк

- **Сохранение:**
  - Создает/обновляет Question
  - Удаляет старые Choice объекты
  - Создает новые Choice для каждого варианта

### NewUserForm (UserCreationForm)
- **Поля:**
  - `username` - имя пользователя
  - `email` - email адрес
  - `password1` - пароль
  - `password2` - подтверждение пароля

- **Сохранение:**
  - Создает пользователя с `is_active=False`
  - Отправляет письмо активации

---

## Views (Представления)

### Публичные views

| URL | View | Метод | Описание |
|-----|------|-------|---------|
| `/polls/` | IndexView | GET | Список последних 5 опросов |
| `/polls/<id>/` | DetailView | GET | Страница голосования |
| `/polls/<id>/results/` | ResultsView | GET | Результаты опроса |
| `/polls/<id>/vote/` | VoteView | POST | Обработка голоса |
| `/polls/login` | LoginView | GET/POST | Вход пользователя |
| `/polls/logout` | LogoutView | GET | Выход пользователя |
| `/polls/register` | AccountRegisterView | GET/POST | Регистрация |
| `/polls/activate/<uidb64>/<token>` | AccountActivationView | GET | Активация аккаунта |

### Защищённые views (только для staff)

| URL | View | Метод | Описание |
|-----|------|-------|---------|
| `/polls/new/` | PollNewView | GET/POST | Создание опроса |
| `/polls/<id>/edit/` | PollEditView | GET/POST | Редактирование опроса |

---

## Система проверки прав доступа

### UserIsStaffMixin
- Проверяет, является ли пользователь администратором (`user.is_staff`)
- Если не администратор → редирект на логин
- Если не залогинен → редирект на логин с параметром `next`

### LoginRequiredMixin
- Проверяет, залогинен ли пользователь
- Если не залогинен → редирект на логин

### Многоуровневое наследование (пример)
```python
class PollNewView(PollsBaseView, UserIsStaffMixin, LoginRequiredMixin, View):
    # Проверяется сначала залогинен ли, потом staff ли
```

---

## Дополнительные функции

### Слоганы
- На каждой странице выводится случайный "страшный" слоган
- Реализовано через `PollsBaseView.get_slogan()`
- 15 различных вариантов

### Система сообщений (Messages)
- Успешный вход: "You are now logged in as {username}"
- Ошибка входа: "Invalid username or password"
- Успешная регистрация: "Registration successful. Please go to your '{email}' e-mail inbox..."
- Успешный выход: "You have successfully logged out"

### Email верификация
- Отправка на email из `settings.EMAIL_HOST_USER`
- Использование `mail.ru` SMTP сервера
- Кодирование ID пользователя в base64
- Создание токена через `default_token_generator`

---

## Конфигурация settings.py

### Важные параметры
```python
DEBUG = True                          # Режим разработки
ALLOWED_HOSTS = []                    # Все хосты
SECRET_KEY = 'django-insecure-...'   # Секретный ключ
```

### Установленные приложения
```python
INSTALLED_APPS = [
    'polls.apps.PollsConfig',
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'crispy_forms',
    'crispy_bootstrap5'
]
```

### Email конфигурация
```python
DEFAULT_FROM_EMAIL = 'polls-from-the-crypt@mail.ru'
EMAIL_BACKEND = 'django.core.mail.backends.smtp.EmailBackend'
EMAIL_HOST = 'smtp.mail.ru'
EMAIL_PORT = 465
EMAIL_USE_SSL = True
EMAIL_HOST_USER = os.environ.get('POLLS_EMAIL_HOST_USER')
EMAIL_HOST_PASSWORD = os.environ.get('POLLS_EMAIL_HOST_PASSWORD')
```

### Crispy Forms
```python
CRISPY_ALLOWED_TEMPLATE_PACKS = "bootstrap5"
CRISPY_TEMPLATE_PACK = "bootstrap5"
```

---

## Технологический стек

| Компонент | Версия | Назначение |
|-----------|--------|-----------|
| Python | 3.13.3 | Язык программирования |
| Django | 5.2.7 | Web фреймворк |
| SQLite3 | - | База данных |
| Bootstrap 5 | 5.1.3 | CSS фреймворк |
| Crispy Forms | 2.4 | Рендеринг форм |
| django-crispy-forms | 2.4 | Интеграция crispy с Django |

---

## Используемые паттерны и практики

### Class-Based Views (CBV)
Все представления наследуют от `View` или `generic.ListView/DetailView` для лучшей организации кода и переиспользования логики.

### Миксины
- `PollsBaseView` - добавляет функционал слоганов
- `UserIsStaffMixin` - проверка прав доступа
- `LoginRequiredMixin` - проверка аутентификации
- `UserPassesTestMixin` - базовая проверка условия

### ModelForm
- `QuestionForm` расширяет функционал стандартной формы
- Кастомная валидация и обработка данных

### Context Processors
Автоматическое добавление пользователя, сообщений и других данных в контекст шаблонов.

---

# Скриншоты приложения Polls

## 1. Главная страница со списком опросов (вход администратора)

![Главная страница с входом администратора](https://github.com/user-attachments/assets/018aa1e4-8c48-4f07-8f91-268fa21179dd)

На главной странице отображается список последних опросов. Администратор может видеть кнопку "Create Poll" для создания новых опросов и иметь доступ к админ-панели.

---

## 2. Форма регистрации нового пользователя

![Регистрация нового пользователя](https://github.com/user-attachments/assets/cb1ad430-8147-4465-ad2e-740429387138)

Форма для регистрации содержит поля: имя пользователя, email, пароль и подтверждение пароля. После заполнения и отправки формы пользователь получит письмо активации на указанный email.

---

## 3. Сообщение об отправке письма активации

![Сообщение об отправке письма активации](https://github.com/user-attachments/assets/c2138260-2b34-4fca-b0a1-43f1286895b1)

После успешной регистрации система выводит сообщение с инструкцией проверить почту и активировать аккаунт. Письмо отправляется на указанный email адрес.

---

## 4. Письмо активации на Gmail

![Письмо активации на Gmail](https://github.com/user-attachments/assets/4fcd166c-f774-41a8-baad-5c42dfc6d326)

Пользователь получает письмо с кнопкой "Activate Account". Ссылка активации содержит закодированный ID пользователя и токен для безопасности.

---

## 5. Страница успешной активации аккаунта

![Успешная активация аккаунта](https://github.com/user-attachments/assets/728719b1-f3dc-4933-89e0-220b22580fbc)

После клика по ссылке активации в письме аккаунт становится активным. Система показывает сообщение "Account Activated!" с кнопкой для входа.

---

## 6. Вход в систему после активации

![Вход в систему после активации](https://github.com/user-attachments/assets/4506411f-96de-48b8-a115-f3922f012b20)

После успешного входа пользователь видит главную страницу со своим профилем (username, email) и может начать голосовать в опросах.

---

