# Auth Service with JWT (workshop4)

Проект представляет собой сервис аутентификации на основе JWT (JSON Web Tokens) с поддержкой ролей пользователей.
Реализованы регистрация, вход, обновление токенов и выход из системы.

## Основные возможности

- Регистрация новых пользователей
- Аутентификация с выдачей access и refresh токенов
- Ролевая модель (ADMIN, PREMIUM_USER, GUEST)
- Защита эндпоинтов с помощью JWT
- Механизм обновления токенов
- Отзыв токенов (logout)

### Запуск приложения

1. Сборка и запуск:

```bash
gradle bootRun
# или
./gradlew bootRun
```

2. Приложение будет доступно по адресу:

```
http://localhost:8080
```

## API Endpoints

### Регистрация пользователя

```http
POST /api/auth/register
Content-Type: application/json

{
  "username": "testuser",
  "password": "Test123!",
  "email": "test@example.com",
  "roles": ["GUEST"]
}
```

### Аутентификация

```http
POST /api/auth/login
Content-Type: application/json

{
  "username": "testuser",
  "password": "Test123!"
}
```

### Обновление токена

```http
POST /api/auth/refresh
Content-Type: application/json
Authorization: Bearer <your_refresh_token>

{
  "refreshToken": "<your_refresh_token>"
}
```

### Выход из системы

```http
POST /api/auth/logout
Content-Type: application/json
Authorization: Bearer <your_access_token>

{
  "refreshToken": "<your_refresh_token>"
}
```

### Пример защищенного запроса

```http
GET /api/admin/dashboard
Authorization: Bearer <your_access_token>
```

## Структура БД

Основные таблицы:

- `users_t1` - информация о пользователях
- `refresh_tokens` - активные refresh-токены
- `user_roles` - роли пользователей

## Ответы

Успешная аутентификация:

```json
{
  "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
  "refreshToken": "f7a83c64-4c2d-4f0a-9b2e-3e4d5f6a7b8c"
}
```

Ошибка:

```json
{
  "timestamp": "2025-07-27T00:00:00.000+00:00",
  "status": 401,
  "error": "Unauthorized",
  "message": "Bad credentials",
  "path": "/api/auth/login"
}
```

## Роли и доступ

| Роль         | Доступ                    |
|--------------|---------------------------|
| ADMIN        | Все эндпоинты             |
| PREMIUM_USER | Базовые + премиум функции |
| GUEST        | Только базовые функции    |
