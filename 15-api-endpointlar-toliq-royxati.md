# 15. API Endpoint'lar To'liq Ro'yxati

Base prefix:

```text
/api
```

## Schema va docs

| Method | Endpoint | Tavsif |
|--------|----------|--------|
| GET | `/` | Swagger UI |
| GET | `/api/schema/` | OpenAPI schema |
| GET | `/api/redoc/` | Redoc |

## Auth

| Method | Endpoint | Auth | Tavsif |
|--------|----------|------|--------|
| POST | `/api/auth/login/` | Ochiq | Login va token olish |
| POST | `/api/auth/logout/` | User | Logout |

## User profile

| Method | Endpoint | Auth | Tavsif |
|--------|----------|------|--------|
| GET | `/api/profile/` | User | Profil |
| PUT | `/api/profile/` | User | Profil update |

## Public data

| Method | Endpoint | Auth | Tavsif |
|--------|----------|------|--------|
| GET | `/api/public/connection/` | Ochiq | Telegram, phone, Instagram, YouTube |
| PUT | `/api/public/connection/` | Admin/Manager `connections` | Aloqa ma'lumotini update |

## User test catalog

| Method | Endpoint | Auth | Tavsif |
|--------|----------|------|--------|
| GET | `/api/themes/` | User | Mavzular ro'yxati |
| GET | `/api/tickets/` | User | Biletlar ro'yxati |
| GET | `/api/coloring/` | User | Ranglash holati |
| PUT | `/api/coloring/` | User | Ranglash update/clear |

## Start tests

| Method | Endpoint | Auth | Body |
|--------|----------|------|------|
| POST | `/api/start_tests/start_theme/` | User | `{ "theme_id": int }` |
| POST | `/api/start_tests/start_ticket/` | User | `{ "ticket_id": int }` |
| POST | `/api/start_tests/start_settest/` | User | `{ "count": int }` |
| POST | `/api/start_tests/start_exam/` | User | `{ "count": int }` |

## Solve tests

| Method | Endpoint | Auth | Tavsif |
|--------|----------|------|--------|
| GET | `/api/result/{result_id}/tests/` | User | Result savollari |
| GET | `/api/result/{result_id}/` | User | Result statistikasi |
| GET | `/api/result/{result_id}/statistics/` | User | Result statistikasi |
| POST | `/api/solve_tests/{testsheet_id}/answer/` | User | Variant yuborish |
| POST | `/api/solve_tests/{result_id}/finish/` | User | Resultni tugatish |

## User statistics and history

| Method | Endpoint | Auth | Tavsif |
|--------|----------|------|--------|
| GET | `/api/statistics/` | User | User umumiy statistika |
| GET | `/api/results/` | User | Resultlar ro'yxati |
| GET | `/api/history/` | User | User tarixi |
| GET | `/api/users/top/` | User | Top 50 umumiy reyting |
| GET | `/api/daily-ranking/` | User | Bugungi reyting |

## AI

| Method | Endpoint | Auth | Tavsif |
|--------|----------|------|--------|
| POST | `/api/ai/chat/` | Kodda ochiq | Test savoli bo'yicha AI javob |

## Telegram account linking

| Method | Endpoint | Auth | Tavsif |
|--------|----------|------|--------|
| POST | `/api/telegram/generate-code/` | Ochiq/internal | 6 xonali kod yaratish |
| GET | `/api/telegram/status/` | User | User uchun aktiv kodni tekshirish |
| POST | `/api/telegram/connect/` | User | Kod orqali Telegramga ulash |
| POST | `/api/telegram/disconnect/` | User | Telegramdan uzish |

## Admin users

| Method | Endpoint | Permission | Tavsif |
|--------|----------|------------|--------|
| GET | `/api/admin/user/` | `users` | User list/filter |
| POST | `/api/admin/user/` | `users` | Student yaratish |
| GET | `/api/admin/user/{id}/` | `users` | User detail |
| PUT | `/api/admin/user/{id}/` | `users` | User update |
| DELETE | `/api/admin/user/{id}/` | `users` | User delete |
| POST | `/api/admin/manager/` | Admin only | Manager yaratish |
| GET | `/api/manager/me/` | `*` | Role, permissions, capabilities |

## Admin themes

| Method | Endpoint | Permission | Tavsif |
|--------|----------|------------|--------|
| GET | `/api/admin/theme/` | `themes` | Theme list |
| POST | `/api/admin/theme/` | `themes` | Theme create |
| GET | `/api/admin/theme/{id}/` | `themes` | Theme detail |
| PUT | `/api/admin/theme/{id}/` | `themes` | Theme update |
| DELETE | `/api/admin/theme/{id}/` | `themes` | Theme delete |

## Admin tickets

| Method | Endpoint | Permission | Tavsif |
|--------|----------|------------|--------|
| GET | `/api/admin/ticket/` | `tickets` | Ticket list |
| POST | `/api/admin/ticket/` | `tickets` | Ticket create |
| GET | `/api/admin/ticket/{id}/` | `tickets` | Ticket detail |
| PUT | `/api/admin/ticket/{id}/` | `tickets` | Ticket update |
| DELETE | `/api/admin/ticket/{id}/` | `tickets` | Ticket delete |

## Admin tests

| Method | Endpoint | Permission | Tavsif |
|--------|----------|------------|--------|
| GET | `/api/admin/test/` | `tests` | Test list |
| GET | `/api/admin/tests/` | `tests` | Test list alias |
| POST | `/api/admin/test/` | `tests` | Test create |
| GET | `/api/admin/test/{id}/` | `tests` | Test detail |
| PUT | `/api/admin/test/{id}/` | `tests` | Test update |
| PATCH | `/api/admin/test/{id}/` | `tests` | Test image upload/remove |
| DELETE | `/api/admin/test/{id}/` | `tests` | Test delete |

## Admin variants

| Method | Endpoint | Permission | Tavsif |
|--------|----------|------------|--------|
| GET | `/api/admin/test/{test_id}/variant/` | `tests` | Variant list |
| POST | `/api/admin/test/{test_id}/variant/` | `tests` | Variant create |
| GET | `/api/admin/test/variant/{id}/` | `tests` | Variant detail |
| PUT | `/api/admin/test/variant/{id}/` | `tests` | Variant update |
| DELETE | `/api/admin/test/variant/{id}/` | `tests` | Variant delete |
| POST | `/api/admin/test/variant/{id}/true/` | `tests` | To'g'ri javob belgilash |

## Admin statistics and results

| Method | Endpoint | Permission | Tavsif |
|--------|----------|------------|--------|
| GET | `/api/admin/statistics/` | `*` | users/themes/tickets/tests count |
| GET | `/api/admin/dashboard-stats/` | `*` | Dashboard full stats |
| GET | `/api/admin/server-info/` | `*` | Server CPU/RAM/disk |
| GET | `/api/admin/all_users_stats/` | `allstats` | Barcha user statistikasi |
| POST | `/api/admin/all_users_stats/` | `allstats` | User EXAM resultlarini o'chirish |
| GET | `/api/admin/user_statistics/{user_id}/` | `users` | Bitta user stats |
| GET | `/api/admin/user_history/{id}/` | `users` | Bitta user history |
| GET | `/api/admin/result_detail/{result_id}/` | `endresults` | Result review |

## Admin media

| Method | Endpoint | Permission | Tavsif |
|--------|----------|------------|--------|
| GET | `/api/admin/media/<path>` | `*` | Media proxy |

