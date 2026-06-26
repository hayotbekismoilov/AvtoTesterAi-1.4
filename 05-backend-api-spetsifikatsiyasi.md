# 5. Backend API Spetsifikatsiyasi

## API bazaviy URL

Frontend default sozlamasi bo'yicha:

```text
https://api.avtotester.uz/api
```

Lokal bot defaulti:

```text
http://127.0.0.1:8011/api
```

## API hujjatlari

| URL | Tavsif |
|-----|--------|
| `/` | Swagger UI (`SpectacularSwaggerView`) |
| `/api/schema/` | OpenAPI schema |
| `/api/redoc/` | Redoc |

## Auth header standarti

Custom token `Authorization` header orqali yuboriladi.

```http
Authorization: <uuid-token>
```

Backend decoratorlar `Token <token>` va `Bearer <token>` formatlarini ham qabul qiladi.

## Response format

Loyiha API'lari asosan JSON qaytaradi.

Muvaffaqiyatli login:

```json
{
  "token": "uuid-token",
  "message": "Login successful",
  "role": "STUDENT",
  "permissions": []
}
```

Xatolik:

```json
{
  "detail": "Invalid token."
}
```

Yoki business error:

```json
{
  "error": "Bu mavzuda testlar mavjud emas"
}
```

## API guruhlari

| Guruh | Prefix | Himoya |
|-------|--------|--------|
| Auth | `/auth/*` | Login ochiq, logout user token |
| User | `/profile`, `/themes`, `/tickets`, `/start_tests`, `/solve_tests` | `user_required` |
| Admin | `/admin/*` | `admin_required` |
| Manager | `/manager/me` | `admin_required(perm="*")` |
| Public | `/public/connection` | GET ochiq, PUT admin/manager |
| Telegram | `/telegram/*` | Generate ochiq/internal, connect/status/disconnect user token |
| AI | `/ai/chat` | Kodda hozir auth decoratorsiz |

## Start test API

### Mavzu bo'yicha

```http
POST /api/start_tests/start_theme/
Authorization: <token>
Content-Type: application/json

{
  "theme_id": 12
}
```

Response:

```json
{
  "id": 1001,
  "user": 5,
  "description": "Theme Yo'l belgilari",
  "test_length": 30,
  "true_answers": 0,
  "incorrect_answers": 0,
  "test_type": "THEME",
  "finished": false
}
```

### Bilet bo'yicha

```http
POST /api/start_tests/start_ticket/

{
  "ticket_id": 3
}
```

### SetTest

```http
POST /api/start_tests/start_settest/

{
  "count": 20
}
```

### Exam

```http
POST /api/start_tests/start_exam/

{
  "count": 20
}
```

Examda student uchun `ruxsat=True` bo'lishi kerak.

## Solve API

### Savollarni olish

```http
GET /api/result/{result_id}/tests/
```

Response elementi:

```json
{
  "id": 123,
  "value": "Savol matni",
  "status": null,
  "image": "https://api.avtotester.uz/media/images/x.webp",
  "current_answer": null,
  "correct_answer": null,
  "variants": [
    {"id": 1, "value": "Variant A", "test": 10}
  ],
  "random_order": false,
  "explanation": "Savol izohi"
}
```

`correct_answer` result tugagandan yoki savolga javob berilgandan keyin ko'rsatiladi.

### Javob yuborish

```http
POST /api/solve_tests/{testsheet_id}/answer/

{
  "variant_id": 55
}
```

Response:

```json
{
  "message": "Javob saqlandi",
  "testsheet_id": 123,
  "correct_answer": {"id": 56, "value": "To'g'ri variant"},
  "successful": false,
  "finished": false
}
```

### Resultni tugatish

```http
POST /api/solve_tests/{result_id}/finish/
```

## Statistics API

| Endpoint | Tavsif |
|----------|--------|
| `GET /api/statistics/` | Userning umumiy statistikasi |
| `GET /api/result/{id}/statistics/` | Bitta result statistikasi |
| `GET /api/results/` | Resultlar ro'yxati; admin uchun oxirgi 50, user uchun oxirgi 20 |
| `GET /api/history/` | Userning oxirgi 100 result tarixi |
| `GET /api/users/top/` | Umumiy top 50 |
| `GET /api/daily-ranking/` | Bugungi top 50 va user rank |

## Admin CRUD API

| Entity | List/Create | Detail |
|--------|-------------|--------|
| User | `GET/POST /api/admin/user/` | `GET/PUT/DELETE /api/admin/user/{id}/` |
| Manager | `POST /api/admin/manager/` | User update orqali permissions |
| Theme | `GET/POST /api/admin/theme/` | `GET/PUT/DELETE /api/admin/theme/{id}/` |
| Ticket | `GET/POST /api/admin/ticket/` | `GET/PUT/DELETE /api/admin/ticket/{id}/` |
| Test | `GET/POST /api/admin/test/` | `GET/PUT/PATCH/DELETE /api/admin/test/{id}/` |
| Variant | `GET/POST /api/admin/test/{test_id}/variant/` | `GET/PUT/DELETE /api/admin/test/variant/{id}/` |

## Upload API

Test rasmi `PATCH /api/admin/test/{id}/` orqali multipart bilan yuklanadi.

Cheklovlar:

| Cheklov | Qiymat |
|---------|--------|
| Maksimal hajm | 5 MB endpoint validatorida |
| Qabul qilinadigan content type | `image/jpeg`, `image/png`, `image/gif`, `image/bmp`, `image/webp` |
| Rasmni o'chirish | `PATCH` ichida `image` bo'lmasa rasm `None` qilinadi |

## AI Chat API

```http
POST /api/ai/chat/

{
  "test_sheet_id": 123,
  "message": "Nima uchun bu xato?",
  "history": [
    {"role": "user", "content": "Oldingi savol"},
    {"role": "assistant", "content": "Oldingi javob"}
  ]
}
```

AI view savol, variantlar, to'g'ri javob, tanlangan xato javob va bazadagi izohdan context quradi.

## Error code xaritasi

| Status | Holat |
|--------|-------|
| 200 | Muvaffaqiyatli GET/PUT/POST |
| 201 | Yaratildi |
| 400 | Validatsiya yoki business xato |
| 401 | Token yo'q yoki invalid |
| 403 | Role/ruxsat yetarli emas yoki expired subscription |
| 404 | Entity topilmadi |
| 500 | AI provider yoki server xatosi |

## API dizayni bo'yicha tavsiyalar

1. Har bir endpoint uchun OpenAPI schema description va request/response serializer qo'shish.
2. `AiChatView`ga `user_required` va `TestSheet` ownership check qo'shish.
3. Admin list endpointlarga paginationni hamma entitylarda bir xil qilish.
4. Error formatni yagona standartga keltirish: `detail`, `code`, `fields`.
5. Token prefixni frontendda ham standart qilish: `Authorization: Token <token>`.

