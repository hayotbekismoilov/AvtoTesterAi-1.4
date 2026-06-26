# 16. Loyiha Fayl Strukturasi

## Root

```text
Avtotester.uz/
|-- BackendAvtotester.uz/
|-- FrontAvtotester.uz/
`-- Malumot/
```

## Backend strukturasi

```text
BackendAvtotester.uz/
|-- api/
|   |-- migrations/
|   |-- views/
|   |   |-- admin_apis.py
|   |   |-- ai_apis.py
|   |   |-- auth_apis.py
|   |   |-- public_apis.py
|   |   |-- telegram_apis.py
|   |   `-- user_apis.py
|   |-- admin.py
|   |-- apps.py
|   |-- decorators.py
|   |-- enums.py
|   |-- models.py
|   |-- serializers.py
|   |-- urls.py
|   `-- utils.py
|-- Bot/
|   |-- config.py
|   |-- handlers.py
|   |-- keyboards.py
|   |-- main.py
|   |-- messages.py
|   `-- requirements.txt
|-- config/
|   |-- asgi.py
|   |-- settings.py
|   |-- urls.py
|   `-- wsgi.py
|-- media/
|-- staticfiles/
|-- db.sqlite3
|-- manage.py
|-- README.md
`-- requirements.txt
```

## Backend fayllar vazifasi

| Fayl | Vazifa |
|------|--------|
| `config/settings.py` | Django settings, DB, CORS, DRF, static/media |
| `config/urls.py` | Schema, Swagger, Redoc, API include, Django admin |
| `api/models.py` | User, Theme, Ticket, Test, Variant, Result, TestSheet, UserSession, Data |
| `api/serializers.py` | User/admin/test serializerlari |
| `api/decorators.py` | `user_required`, `admin_required` |
| `api/enums.py` | Role, test type, manager capabilities |
| `api/urls.py` | Barcha API route'lar |
| `api/views/user_apis.py` | User test, result, statistics, coloring, ranking |
| `api/views/admin_apis.py` | Admin CRUD, dashboard, media, manager |
| `api/views/auth_apis.py` | Login/logout |
| `api/views/public_apis.py` | Public connection data |
| `api/views/telegram_apis.py` | Telegram code connect/disconnect |
| `api/views/ai_apis.py` | AI chat |
| `api/admin.py` | Django admin customization |
| `api/utils.py` | Token generate |

## Frontend strukturasi

```text
FrontAvtotester.uz/
|-- public/
|   |-- car.png
|   `-- icons.png
|-- src/
|   |-- assets/
|   |   `-- images/test.jpg
|   |-- components/
|   |   |-- Layout/
|   |   |   |-- Header.tsx
|   |   |   `-- Layout.tsx
|   |   `-- Ui/
|   |       |-- ColoringModal.tsx
|   |       `-- CustomAlert.tsx
|   |-- contexts/
|   |   |-- AlertContext.tsx
|   |   `-- ThemeContext.tsx
|   |-- pages/
|   |   |-- Admin/
|   |   |-- Auth/
|   |   |-- Dashboard/
|   |   |-- About/
|   |   |-- Others/
|   |   `-- profile/
|   |-- routes/
|   |   `-- AppRoutes.tsx
|   |-- utils/
|   |   |-- Admin.tsx
|   |   |-- Backend.tsx
|   |   |-- coloring.ts
|   |   `-- latinToCyrillic.tsx
|   |-- App.css
|   |-- App.tsx
|   |-- index.css
|   `-- main.tsx
|-- dist/
|-- package.json
|-- vite.config.ts
|-- tsconfig.json
`-- README.md
```

## Frontend pages

| Papka | Sahifalar |
|-------|-----------|
| `Auth` | Login, Unauthenticated |
| `Dashboard` | Dashboard, Solve, Statistics, TopUsers, DailyTop, TestResult |
| `Dashboard/ByTheme` | Mavzu bo'yicha test |
| `Dashboard/ByTicket` | Bilet bo'yicha test |
| `Dashboard/SetTests` | Sozlamali test |
| `Dashboard/Exam` | Auto exam start |
| `Dashboard/History` | History va review |
| `Admin/components` | User/Test/Theme/Ticket/Stats/Connections management |
| `Admin/History` | Admin user history va result review |
| `profile` | Profile, Settings, Developer |
| `Others` | Connections, NotFound |

## Bot strukturasi

```text
BackendAvtotester.uz/Bot/
|-- config.py
|-- handlers.py
|-- keyboards.py
|-- main.py
|-- messages.py
`-- requirements.txt
```

| Fayl | Vazifa |
|------|--------|
| `main.py` | Bot, Dispatcher, polling |
| `config.py` | BOT_TOKEN, URLs, ADMIN_IDS |
| `handlers.py` | Komandalar va callbacklar |
| `keyboards.py` | Inline keyboardlar |
| `messages.py` | HTML xabar template'lari |

## Hujjatlar strukturasi

```text
Malumot/
|-- README.md
|-- 01-loyiha-umumiy-malumoti.md
|-- 02-texnologik-stek.md
|-- ...
`-- 17-kelajakdagi-rivojlantirish-roadmap.md
```

## Git holati bo'yicha eslatma

Repo ishchi holatida ko'p media fayllar `deleted/untracked`, ayrim backend/frontend fayllar esa modified ko'rinmoqda. Ushbu hujjatlar faqat `Malumot/` papkasiga qo'shildi; mavjud kod yoki media fayllar qaytarilmadi va o'zgartirilmadi.

## Strukturani tozalash tavsiyalari

1. `node_modules/` va `dist/` gitdan chiqarilganini tekshirish.
2. Media fayllarni gitga kiritmaslik uchun `.gitignore`ni tiklash.
3. Backend va frontend uchun alohida `.env.example`larni to'liq yangilash.
4. Docs papkasini `Malumot/` yoki `docs/` sifatida standartlashtirish.
5. Katta komponentlarni modul bo'yicha mayda fayllarga ajratish.
