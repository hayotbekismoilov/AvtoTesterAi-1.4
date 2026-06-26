# 2. Texnologik Stek

## Umumiy stek

| Qatlam | Texnologiya | Loyiha ichidagi roli |
|--------|-------------|----------------------|
| Frontend | React, TypeScript, Vite | Mobile Web App va admin/manager panel |
| Styling | Tailwind CSS 4, custom CSS tokens | Responsive dark/light UI, admin theme |
| Routing | React Router DOM | Public, user, admin va manager route'lar |
| Icons | lucide-react | UI ikonkalari |
| Charts | Recharts | Admin dashboard va statistik grafiklar |
| Backend | Django 5.2.7 | API, model, admin, static/media |
| API | Django REST Framework 3.16.1 | REST endpointlar |
| API docs | drf-spectacular | OpenAPI schema, Swagger, Redoc |
| DB | SQLite | Hozirgi lokal/production settingsda asosiy DB |
| Bot | Aiogram 3.7.0 | Telegram bot va Mini App tugmalari |
| AI | OpenRouter API | Test savoli bo'yicha AI chat |

## Frontend paketlari

`FrontAvtotester.uz/package.json` asosida:

| Paket | Versiya | Izoh |
|-------|---------|------|
| `react` | `^19.1.1` | UI komponentlar |
| `react-dom` | `^19.1.1` | DOM render |
| `react-router-dom` | `^7.9.5` | Route boshqaruvi |
| `typescript` | `~5.9.3` | Type safety |
| `vite` | `^7.1.7` | Dev server va build |
| `@vitejs/plugin-react` | `^5.0.4` | React plugin |
| `tailwindcss` | `^4.1.16` | Utility CSS |
| `@tailwindcss/vite` | `^4.1.16` | Tailwind Vite integratsiyasi |
| `lucide-react` | `^0.553.0` | Ikonkalar |
| `recharts` | `^3.8.1` | Grafiklar |
| `eslint` | `^9.36.0` | Lint |

## Backend paketlari

`BackendAvtotester.uz/requirements.txt` asosida:

| Paket | Versiya | Izoh |
|-------|---------|------|
| `Django` | `5.2.7` | Asosiy web framework |
| `djangorestframework` | `3.16.1` | REST API |
| `django-cors-headers` | `4.9.0` | CORS sozlamalari |
| `drf-spectacular` | `0.29.0` | OpenAPI schema |
| `drf-yasg` | `1.21.11` | Swagger ekotizimi uchun qo'shimcha |
| `pillow` | `12.0.0` | ImageField va rasm ishlovi |
| `python-dotenv` | `1.2.1` | `.env` o'qish |
| `pytz` | `2025.2` | Asia/Tashkent kunlik reyting |
| `gunicorn` | `23.0.0` | Production WSGI server |
| `psycopg2-binary` | `2.9.10` | Postgresga o'tish uchun tayyor paket |
| `google-genai` | `1.3.0` | Hozirgi AI view OpenRouter ishlatadi; paket mavjud |

## Telegram bot paketlari

`BackendAvtotester.uz/Bot/requirements.txt` asosida:

| Paket | Versiya | Izoh |
|-------|---------|------|
| `aiogram` | `3.7.0` | Bot framework |
| `python-dotenv` | `1.0.0` | Bot env sozlamalari |
| `aiohttp` | `>=3.9.0` | Backend API bilan async HTTP |

## Runtime va muhit

| Qism | Talab |
|------|-------|
| Python | 3.11+ tavsiya qilinadi |
| Node.js | 18+ tavsiya qilinadi |
| OS | Linux server uchun qulay |
| Timezone | Backend `Asia/Tashkent` |
| Static | Django `STATIC_ROOT = BASE_DIR / "static"` |
| Media | Django `MEDIA_ROOT = BASE_DIR / "media"` |

## Muhim integratsiyalar

| Integratsiya | Koddagi joyi | Tavsif |
|--------------|--------------|--------|
| Telegram WebApp | `FrontAvtotester.uz/src/App.tsx` | `ready()`, `expand()`, BackButton, haptic feedback |
| Bot Mini App | `BackendAvtotester.uz/Bot/keyboards.py` | `WebAppInfo(url=FRONTEND_URL)` |
| OpenRouter | `BackendAvtotester.uz/api/views/ai_apis.py` | Chat completions API |
| DRF schema | `BackendAvtotester.uz/config/urls.py` | `/api/schema/`, `/`, `/api/redoc/` |
| Media proxy | `BackendAvtotester.uz/api/views/admin_apis.py` | `/api/admin/media/<path>` |

## Stek tanlovining sababi

| Tanlov | Afzallik |
|--------|----------|
| React + Vite | Mobil UI tez ishlab chiqiladi, Telegram WebAppga mos |
| Django + DRF | Model, admin, API va auth bir ekotizimda |
| SQLite | Soddalik va tez start; kichik/orta yuklama uchun qulay |
| Aiogram | Telegram botni async yozish va Mini App tugmalarini qulay boshqarish |
| Recharts | Admin statistikani tez vizualizatsiya qilish |

## E'tibor kerak bo'lgan joylar

| Masala | Tavsiya |
|--------|---------|
| SQLite hajmi | 2.2M+ `TestSheet` yozuvi bor; Postgresga o'tish rejasini tayyorlash kerak |
| Secret handling | Bot token default qiymat sifatida kodda turibdi; faqat env orqali o'qilishi kerak |
| CORS | Productionda originlarni whitelist qilish kerak |
| Type mismatch | Backend role `STUDENT`, frontend type `USER/GUEST` ham bor; role mappingni aniq standartlashtirish kerak |

