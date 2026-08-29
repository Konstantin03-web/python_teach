# Сложный уровень - Базовый мессенджер на Flask

## Цель проекта
Разработать базовый веб-мессенджер с использованием Flask(либо django, fastapi), Jinja2 шаблонов и минимальным JavaScript для WebSockets.

## Общее описание
Проект представляет собой веб-приложение, позволяющее пользователям обмениваться сообщениями в реальном времени. Основной упор делается на серверную часть и работу с шаблонами Jinja2, а не на сложный фронтенд.

## Требования к проекту

### Функциональные требования

#### 1. Аутентификация и авторизация
- Регистрация нового пользователя
  - Поля: username, email, password (минимум 6 символов)
  - Проверка на уникальность username и email
  - Хеширование паролей (Flask-Bcrypt или werkzeug.security)
- Вход в систему (login)
  - Проверка правильности введенных данных
  - Использование Flask-Login для управления сессиями
- Выход из системы (logout)
- Защита маршрутов (декоратор @login_required)
- Отображение статуса пользователя (онлайн/оффлайн)

#### 2. Профиль пользователя
- Просмотр профиля
  - Имя пользователя
  - Email
  - Дата регистрации
  - Статус (онлайн/оффлайн)
- Редактирование профиля
  - Смена имени
  - Смена пароля (с подтверждением)
  - Загрузка аватара (опционально, через Flask-Uploads)
- Список всех пользователей системы
  - Поиск по имени (через форму на странице)

#### 3. Управление чатами
- Создание нового чата
  - Выбор пользователя из списка
  - Переход в чат после создания
- Список всех чатов пользователя
  - Сортировка по дате последнего сообщения
  - Отображение последнего сообщения
  - Отображение количества непрочитанных сообщений
- Удаление чата (только для создателя)
- Отображение информации о чате

#### 4. Сообщения
- Отправка текстовых сообщений
  - Максимальная длина: 1000 символов
  - Поддержка эмодзи
- Отображение истории сообщений в чате
  - Подгрузка по 20 сообщений (пагинация)
  - Отображение времени отправки
  - Отображение аватара отправителя
- Получение сообщений в реальном времени
  - Использование Flask-SocketIO
- Автоматическая прокрутка к новым сообщениям
- Отметка о прочтении (опционально)

#### 5. Пользовательский интерфейс
- Единый базовый шаблон (base.html) с навигацией
- Использование Bootstrap для стилизации
- Адаптивный дизайн
- Две основные области:
  - Левая панель: список чатов (включая поиск)
  - Правая панель: окно текущего чата
- Интуитивная навигация через меню
- Обработка ошибок с понятными сообщениями (flash-сообщения)

### Технические требования

#### Обязательные технологии:
- **Flask** - веб-фреймворк
- **Flask-SocketIO** - для WebSockets
- **Flask-SQLAlchemy** - ORM для работы с БД
- **Flask-Login** - управление сессиями
- **Jinja2** - шаблонизатор (встроен в Flask)
- **Bootstrap 5** - CSS-фреймворк (подключается через CDN)
- **SQLite** - база данных (для разработки)

#### Рекомендуемые расширения:
- **Flask-WTF** - для работы с формами
- **Flask-Bcrypt** - для хеширования паролей
- **python-dotenv** - для управления переменными окружения
- **Flask-Migrate** - для управления миграциями БД

### Структура проекта
```
├── app/
│ ├── init.py
│ ├── models.py # Модели базы данных
│ ├── routes.py # Маршруты (основные)
│ ├── auth.py # Маршруты для аутентификации
│ ├── forms.py # Формы WTForms
│ ├── extensions.py # Инициализация расширений
│ ├── socket_handlers.py # Обработчики WebSocket
│ └── utils.py # Вспомогательные функции
├── templates/
│ ├── base.html # Базовый шаблон с навигацией
│ ├── login.html # Страница входа
│ ├── register.html # Страница регистрации
│ ├── profile.html # Страница профиля
│ ├── edit_profile.html # Редактирование профиля
│ ├── chat.html # Главная страница с чатами
│ ├── users.html # Список пользователей
│ └── chat_room.html # Окно чата (включается в chat.html)
├── static/
│ ├── css/
│ │ └── style.css # Пользовательские стили
│ └── js/
│ └── chat.js # Минимальный JS для WebSockets
├── migrations/ # Папка для миграций (Flask-Migrate)
├── uploads/ # Папка для загруженных аватаров
├── config.py # Конфигурация приложения
├── run.py # Точка входа (запуск приложения)
├── requirements.txt # Зависимости
├── .env # Переменные окружения
└── README.md # Документация проекта
```


### Модели базы данных (пример)

```python
# app/models.py

from app.extensions import db
from flask_login import UserMixin
from datetime import datetime

class User(db.Model, UserMixin):
    __tablename__ = 'users'
    
    id = db.Column(db.Integer, primary_key=True)
    username = db.Column(db.String(80), unique=True, nullable=False)
    email = db.Column(db.String(120), unique=True, nullable=False)
    password_hash = db.Column(db.String(200), nullable=False)
    avatar = db.Column(db.String(200), default='default.png')
    status = db.Column(db.String(50), default='offline')
    last_seen = db.Column(db.DateTime, default=datetime.utcnow)
    created_at = db.Column(db.DateTime, default=datetime.utcnow)
    
    # Отношения
    messages = db.relationship('Message', backref='sender', lazy=True)
    chats = db.relationship('ChatParticipant', backref='user', lazy=True)
    
    def __repr__(self):
        return f'<User {self.username}>'

class Chat(db.Model):
    __tablename__ = 'chats'
    
    id = db.Column(db.Integer, primary_key=True)
    name = db.Column(db.String(100), nullable=True)
    is_group = db.Column(db.Boolean, default=False)
    created_at = db.Column(db.DateTime, default=datetime.utcnow)
    creator_id = db.Column(db.Integer, db.ForeignKey('users.id'))
    
    # Отношения
    participants = db.relationship('ChatParticipant', backref='chat', lazy=True)
    messages = db.relationship('Message', backref='chat', lazy=True)
    
    def __repr__(self):
        return f'<Chat {self.id}>'

class ChatParticipant(db.Model):
    __tablename__ = 'chat_participants'
    
    id = db.Column(db.Integer, primary_key=True)
    chat_id = db.Column(db.Integer, db.ForeignKey('chats.id'))
    user_id = db.Column(db.Integer, db.ForeignKey('users.id'))
    joined_at = db.Column(db.DateTime, default=datetime.utcnow)
    last_read = db.Column(db.DateTime, default=datetime.utcnow)

class Message(db.Model):
    __tablename__ = 'messages'
    
    id = db.Column(db.Integer, primary_key=True)
    chat_id = db.Column(db.Integer, db.ForeignKey('chats.id'))
    sender_id = db.Column(db.Integer, db.ForeignKey('users.id'))
    content = db.Column(db.Text, nullable=False)
    timestamp = db.Column(db.DateTime, default=datetime.utcnow)
    is_read = db.Column(db.Boolean, default=False)
    
    def __repr__(self):
        return f'<Message {self.id}>'
```

