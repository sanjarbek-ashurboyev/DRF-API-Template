# DRF API Template

A simple starter template for building REST APIs with Django and Django REST Framework.

## What's included

- **Django 6.1** + **Django REST Framework**
- **JWT auth** with [simplejwt](https://github.com/jazzband/djangorestframework-simplejwt): register, login, refresh, current user
- **Custom User model**, ready to extend
- **API docs** with [drf-spectacular](https://github.com/tfranzel/drf-spectacular) (Swagger UI)
- **CORS** with [django-cors-headers](https://github.com/adamchainz/django-cors-headers)
- **Settings from environment variables** (`.env`)
- **Tests** and a **GitHub Actions** CI workflow

## Quick start

```bash
git clone https://github.com/<your-username>/<your-repo>.git
cd <your-repo>

python -m venv .venv
source .venv/bin/activate        # Windows: .venv\Scripts\activate
pip install -r requirements.txt

cp .env.example .env             # then edit the values
python manage.py migrate
python manage.py createsuperuser
python manage.py runserver
```

Open http://127.0.0.1:8000/api/docs/ to see the API docs.

## Endpoints

| Method      | URL                       | Auth | Description               |
|-------------|---------------------------|------|---------------------------|
| GET         | `/api/health/`            | No   | Health check              |
| POST        | `/api/v1/auth/register/`  | No   | Create an account         |
| POST        | `/api/v1/auth/login/`     | No   | Get access + refresh JWT  |
| POST        | `/api/v1/auth/refresh/`   | No   | Get a new access token    |
| GET / PATCH | `/api/v1/auth/me/`        | Yes  | View / update current user |
| GET         | `/api/docs/`              | No   | Swagger UI                |
| GET         | `/api/schema/`            | No   | OpenAPI schema            |
| -           | `/admin/`                 | -    | Django admin              |

Send the access token in the header of authenticated requests:

```
Authorization: Bearer <access_token>
```

## Project structure

```
.
├── apps/
│   └── users/          # Custom user model + auth endpoints
├── root/
│   ├── settings.py     # All settings, read from .env
│   ├── urls.py         # Root URL config
│   └── views.py        # Health check
├── .env.example
├── manage.py
└── requirements.txt
```

## Adding a new app

```bash
mkdir apps/<name>
python manage.py startapp <name> apps/<name>
```

Then:

1. In `apps/<name>/apps.py`, set `name = 'apps.<name>'`.
2. Add `'apps.<name>'` to `INSTALLED_APPS` in `root/settings.py`.
3. Include its URLs in `root/urls.py`, e.g. `path('api/v1/<name>/', include('apps.<name>.urls'))`.

## Running tests

```bash
python manage.py test
```

## Environment variables

| Variable               | Default                  | Description                          |
|------------------------|--------------------------|--------------------------------------|
| `SECRET_KEY`           | insecure dev key         | **Set this in production**           |
| `DEBUG`                | `False`                  | Turn on debug mode                   |
| `ALLOWED_HOSTS`        | `localhost,127.0.0.1`    | Comma-separated hosts                |
| `CORS_ALLOWED_ORIGINS` | empty                    | Comma-separated frontend origins     |
| `JWT_ACCESS_MINUTES`   | `30`                     | Access token lifetime                |
| `JWT_REFRESH_DAYS`     | `7`                      | Refresh token lifetime               |

The template uses SQLite. To switch to PostgreSQL, install `psycopg` and change `DATABASES` in `root/settings.py`.

## License

[MIT](LICENSE)
