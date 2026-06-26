# 3. Tizim Arxitekturasi

## Yuqori darajadagi ko'rinish

```mermaid
flowchart LR
    U[User / Student] --> F[React Mobile Web App]
    A[Admin / Manager] --> F
    T[Telegram User] --> B[Telegram Bot]
    B --> F
    B --> API[Django REST API]
    F --> API
    API --> DB[(SQLite DB)]
    API --> M[Media Storage]
    API --> O[OpenRouter AI]
    API --> DA[Django Admin]
```

## Modul chegaralari

| Modul | Papka | Mas'uliyat |
|-------|-------|------------|
| Backend config | `BackendAvtotester.uz/config` | URL, settings, WSGI/ASGI |
| API app | `BackendAvtotester.uz/api` | Model, serializer, view, decorator, admin |
| Bot | `BackendAvtotester.uz/Bot` | Telegram komandalar, Mini App tugmalari, account linking |
| Frontend app | `FrontAvtotester.uz/src` | User UI, admin UI, manager UI |
| Media | `BackendAvtotester.uz/media` | Test rasmlari va Telegram avatarlar |
| Static | `BackendAvtotester.uz/staticfiles`, `static` | DRF/admin static fayllar |

## Backend ichki arxitekturasi

```mermaid
flowchart TD
    URL[api/urls.py] --> AUTH[auth_apis.py]
    URL --> USER[user_apis.py]
    URL --> ADMIN[admin_apis.py]
    URL --> PUBLIC[public_apis.py]
    URL --> TG[telegram_apis.py]
    URL --> AI[ai_apis.py]
    AUTH --> DEC[decorators.py]
    USER --> DEC
    ADMIN --> DEC
    AUTH --> MODELS[models.py]
    USER --> MODELS
    ADMIN --> MODELS
    TG --> MODELS
    MODELS --> DB[(db.sqlite3)]
    USER --> SER[serializers.py]
    ADMIN --> SER
```

## Frontend ichki arxitekturasi

```mermaid
flowchart TD
    MAIN[main.tsx] --> APP[App.tsx]
    APP --> ROUTES[AppRoutes.tsx]
    APP --> THEME[ThemeContext]
    APP --> ALERT[AlertContext]
    ROUTES --> USERP[Dashboard pages]
    ROUTES --> ADMINP[AdminDashboard]
    ROUTES --> MANAGERP[ManagerDashboard]
    USERP --> BACKEND[utils/Backend.tsx]
    ADMINP --> ADMINAPI[utils/Admin.tsx]
    MANAGERP --> ADMINAPI
```

## Request lifecycle

### Oddiy user login

```mermaid
sequenceDiagram
    participant F as Frontend
    participant API as Django API
    participant DB as DB

    F->>API: POST /api/auth/login/
    API->>DB: authenticate user
    API->>DB: UserSession create/update
    API-->>F: token, role, permissions
    F->>F: localStorage token cache
```

### Mavzu bo'yicha test boshlash

```mermaid
sequenceDiagram
    participant F as Frontend
    participant API as Django API
    participant DB as DB

    F->>API: POST /api/start_tests/start_theme/
    API->>DB: Theme va active Testlarni olish
    API->>DB: eski unfinished Resultlarni finish qilish
    API->>DB: Result yaratish
    API->>DB: har bir Test uchun TestSheet yaratish
    API-->>F: Result data
    F->>F: /testresult/:result_id ga o'tish
```

### Javob yuborish

```mermaid
sequenceDiagram
    participant F as Frontend
    participant API as Django API
    participant DB as DB

    F->>API: POST /api/solve_tests/:testsheet_id/answer/
    API->>DB: TestSheet va Variant tekshirish
    API->>DB: successful=true/false saqlash
    API->>DB: Result true/incorrect counter update
    API->>DB: exam timeout yoki 3 xato tekshirish
    API-->>F: correct_answer, successful, finished
```

## Muhim data flow'lar

| Flow | Kirish | Chiqish | Muhim qoida |
|------|--------|---------|-------------|
| Login | username/password | custom UUID token | bir userga bitta `UserSession` |
| Start test | theme/ticket/count | `Result` | eski active sessiyalar yakunlanadi |
| Get tests | result_id | `TestSheet[]` | variant tartibi `variant_orders` orqali saqlanadi |
| Answer | testsheet_id + variant_id | correct/wrong | bir `TestSheet` faqat bir marta tanlanadi |
| Finish | result_id | final score | countlar `TestSheet`dan qayta hisoblanadi |
| Telegram connect | 6 xonali code | user.telegram_id | kod 10 daqiqa ichida amal qiladi |
| Admin CRUD | admin token | entity create/update/delete | manager permission bilan cheklanadi |

## Tizimdagi asosiy shartlar

| Shart | Amalga oshirilgan joy |
|-------|-----------------------|
| Foydalanuvchi tokeni `Authorization` headerda yuboriladi | `utils/Backend.tsx`, `decorators.py` |
| Admin/manager endpointlar `admin_required` orqali himoyalanadi | `decorators.py` |
| User endpointlar `user_required` orqali himoyalanadi | `decorators.py` |
| Django admin alohida `/admin/` URLda | `config/urls.py` |
| React admin panel `/admin/*` route'larda | `AppRoutes.tsx` |
| Manager panel `/manager/*` route'larda | `AppRoutes.tsx` |

## Arxitektura kuchli tomonlari

- Model va business logic backendda markazlashgan.
- Frontend user/admin/manager oqimlari route darajasida ajratilgan.
- Manager ruxsatlari backend va frontendda bir xil capability keylarga tayanadi.
- Bot web ilovani takrorlamaydi, balki Mini Appga kirish eshigi sifatida ishlaydi.
- Result va TestSheet ajratilgani sababli review, history va statistikani qurish oson.

## Arxitektura risklari

| Risk | Sabab | Tavsiya |
|------|-------|---------|
| DB hajmi ortishi | `TestSheet` soni juda katta | Postgres, indekslar, archive strategiyasi |
| API permission drift | Ba'zi endpointlarda user/admin decorator yo'q yoki open | Har bir endpoint uchun permission matrix test |
| Frontend token cache | localStorage ishlatiladi | XSS himoyasi, CSP va token rotatsiya |
| CORS keng | `CORS_ALLOW_ALL_ORIGINS=True` | Production whitelist |

