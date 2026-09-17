# Task Manager API

A RESTful API built with Django REST Framework for managing personal tasks, featuring token-based authentication and per-user data isolation.

## Features

- **Full CRUD** — Create, read, update, and delete tasks
- **Token Authentication** — Secure endpoints, each user only accesses their own data
- **Filtering** — Filter tasks by `status` and `due_date`
- **Admin panel** — Manage users and tasks via Django's built-in admin interface

## Tech Stack

- Python
- Django
- Django REST Framework
- django-filter
- SQLite (development database)

## Project Structure

```
task-manager-api/
├── manage.py
├── taskmanager/        # Project settings & root URLs
│   ├── settings.py
│   └── urls.py
└── tasks/               # Tasks app
    ├── models.py        # Task model
    ├── serializers.py   # Task serializer
    ├── views.py         # TaskViewSet (CRUD logic)
    └── urls.py           # API routes
```

## Getting Started

### 1. Clone the repository

```bash
git clone https://github.com/Safae-ElHamri/task-manager-api.git
cd task-manager-api
```

### 2. Create a virtual environment and install dependencies

```bash
python3 -m venv venv
source venv/bin/activate   # On Windows: venv\Scripts\activate
pip install django djangorestframework django-filter
```

### 3. Apply migrations

```bash
python manage.py makemigrations
python manage.py migrate
```

### 4. Create a superuser

```bash
python manage.py createsuperuser
```

### 5. Run the server

```bash
python manage.py runserver
```

The API is now available at `http://127.0.0.1:8000/api/tasks/`.

## Authentication

This API uses **Token Authentication**. To get a token for a user, generate one via the Django shell:

```python
python manage.py shell

from django.contrib.auth.models import User
from rest_framework.authtoken.models import Token

user = User.objects.get(username='your_username')
token, created = Token.objects.get_or_create(user=user)
print(token.key)
```

Include the token in the `Authorization` header of every request:

```
Authorization: Token <your_token_here>
```

## API Endpoints

| Method | Endpoint            | Description                     |
|--------|----------------------|----------------------------------|
| GET    | `/api/tasks/`         | List all tasks for the authenticated user |
| POST   | `/api/tasks/`         | Create a new task               |
| GET    | `/api/tasks/{id}/`    | Retrieve a single task          |
| PUT    | `/api/tasks/{id}/`    | Update a task                   |
| DELETE | `/api/tasks/{id}/`    | Delete a task                   |

### Filtering

```
GET /api/tasks/?status=todo
GET /api/tasks/?due_date=2026-09-20
```

## Example Request

```bash
curl -X POST http://127.0.0.1:8000/api/tasks/ \
  -H "Authorization: Token <your_token>" \
  -H "Content-Type: application/json" \
  -d '{"title": "Learn Django REST Framework", "status": "todo"}'
```

**Response:**

```json
{
  "id": 1,
  "owner": "your_username",
  "title": "Learn Django REST Framework",
  "description": "",
  "status": "todo",
  "due_date": null,
  "created_at": "2026-09-17T11:54:20.679524Z"
}
```

## What I Learned

Building this project helped me apply core backend concepts in practice:

- Structuring a Django project with models, serializers, and viewsets
- Implementing token-based authentication and per-user permissions
- Using `ModelViewSet` to build a complete CRUD API with minimal code
- Adding filtering with `django-filter`
- Debugging real-world issues (migrations, environment/interpreter mismatches, authentication flows)

## Author

**Safae El Hamri**
[LinkedIn](https://www.linkedin.com/in/safae-el-hamri-19134b387) · [GitHub](https://github.com/Safae-ElHamri)
