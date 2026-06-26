# 9. Admin Panel

## Admin panel vazifasi

Admin panel tizimdagi foydalanuvchilar, test kontenti, variantlar, rasmlar, natijalar, statistikalar va aloqa ma'lumotlarini boshqaradi. Panel React ichida `/admin/*` route'larda ishlaydi, Django admin esa alohida `/admin/` URLda mavjud.

## Panel turlari

| Panel | Route | Kim kiradi |
|-------|-------|------------|
| React Admin Panel | `/admin/*` | `ADMIN` |
| React Manager Panel | `/manager/*` | `MANAGER` yoki `ADMIN` |
| Django Admin | `/admin/` backend URL | staff/superuser |

## React admin komponentlari

| Component | Vazifa |
|-----------|--------|
| `AdminDashboard` | Shell, sidebar, topbar, dashboard |
| `UserManagement` | Foydalanuvchilar CRUD |
| `TestsManagement` | Testlar, variantlar, rasmlar, izohlar |
| `ThemeManagement` | Mavzular CRUD |
| `TicketManagement` | Biletlar CRUD |
| `UsersStatisticsManagement` | Foydalanuvchi statistikasi |
| `EndResults` | Resultlar ro'yxati |
| `ConnectionsManagement` | Public aloqa linklari |
| `AdminUserHistory` | Bitta user tarixi |
| `AdminHistoryReview` | Bitta result batafsil review |

## Navigation bo'limlari

| ID | Label | Manager permission |
|----|-------|--------------------|
| `dashboard` | Dashboard | `*` |
| `users` | Foydalanuvchilar | `users` |
| `allstats` | Statistika | `allstats` |
| `endresults` | Natijalar | `endresults` |
| `themes` | Mavzular | `themes` |
| `tickets` | Biletlar | `tickets` |
| `tests` | Testlar | `tests` |
| `connections` | Murojatlar | `connections` |

## Dashboard API

Endpoint:

```http
GET /api/admin/dashboard-stats/
```

Response bloklari:

| Blok | Ma'lumot |
|------|----------|
| `totals` | users, tests, themes, tickets, results |
| `today` | bugungi total, passed, users, pass rate, delta |
| `weekly` | oxirgi 7 kun chart |
| `donut` | passed/failed |
| `type_breakdown` | test_type bo'yicha natijalar |
| `recent` | oxirgi 8 result |
| `top_users` | top 5 |
| `server` | CPU, RAM, disk |

## User management

Endpointlar:

| Method | Endpoint | Vazifa |
|--------|----------|--------|
| GET | `/api/admin/user/` | User list, filter, pagination |
| POST | `/api/admin/user/` | Student yaratish |
| GET | `/api/admin/user/{id}/` | User detail |
| PUT | `/api/admin/user/{id}/` | User update |
| DELETE | `/api/admin/user/{id}/` | User delete |
| POST | `/api/admin/manager/` | Manager yaratish |
| GET | `/api/admin/user_statistics/{id}/` | Bitta user statistikasi |
| GET | `/api/admin/user_history/{id}/` | Bitta user tarixi |

User list query paramlari:

| Param | Tavsif |
|-------|--------|
| `search` | username/full_name/telegram_username |
| `role` | ADMIN/MANAGER/STUDENT |
| `status` | ruxsat true/false |
| `sort_field` | id, username, full_name, role, ruxsat va boshqalar |
| `sort_dir` | asc/desc |
| `page` | sahifa |
| `page_size` | 10 dan 200 gacha |

## Test management

Test yaratish:

```http
POST /api/admin/test/
Content-Type: multipart/form-data
```

Muhim fieldlar:

| Field | Izoh |
|-------|------|
| `value` | Savol |
| `ticket` | Majburiy |
| `theme` | Ixtiyoriy |
| `random_order` | Variantlarni random ko'rsatish |
| `explanation` | Savol izohi |
| `image` | Ixtiyoriy rasm |

Variant yaratish:

```http
POST /api/admin/test/{test_id}/variant/
```

To'g'ri javob belgilash:

```http
POST /api/admin/test/variant/{variant_id}/true/
```

Bu endpoint:

1. `variant.test.correct_answer = variant` qiladi.
2. `variant.test.active = True` qiladi.
3. Testni saqlaydi.

## Rasm boshqaruvi

| Amal | Endpoint |
|------|----------|
| Upload/replace | `PATCH /api/admin/test/{id}/` multipart `image` |
| Delete image | `PATCH /api/admin/test/{id}/` image yubormasdan |
| Proxy orqali ko'rish | `GET /api/admin/media/<path>` |

## Mavzu va bilet boshqaruvi

| Entity | List/Create | Detail |
|--------|-------------|--------|
| Theme | `/api/admin/theme/` | `/api/admin/theme/{id}/` |
| Ticket | `/api/admin/ticket/` | `/api/admin/ticket/{id}/` |

## Natijalar va review

| Endpoint | Tavsif |
|----------|--------|
| `GET /api/results/` | Admin uchun oxirgi 50 result |
| `GET /api/admin/result_detail/{result_id}/` | Result, tests, current/correct answers |
| `GET /api/admin/user_history/{user_id}/` | User tarixini ko'rish |
| `POST /api/admin/all_users_stats/` | Userning EXAM resultlarini tozalash |

## Django admin

Django admin quyidagi modellarni ko'rsatadi:

| Model | Admin imkoniyat |
|-------|-----------------|
| User | Role, ruxsat, status filter |
| Theme | Inline tests va muammoli test statistikasi |
| Ticket | Inline tests |
| Test | Variant inline, activate/deactivate actions |
| Variant | To'g'ri javob badge |
| Result | Score display |
| UserSession | Token preview |
| TestSheet | Status badge |
| BrokenTestProxy | `correct_answer=NULL` testlarni alohida ko'rish |

## Admin panel risklari

| Risk | Izoh | Tavsiya |
|------|------|---------|
| Superuser update cheklovi | `is_staff` userlarni update/delete qilish rad etiladi | Yaxshi, test bilan mustahkamlash |
| Manager permission | `panel_perm`ga tayanadi | Har endpoint uchun permission matrix test |
| File upload | 5MB va content-type check bor | MIME sniffing va virus scan qo'shish mumkin |
| Server specs | CPU/RAM/DISK hardcoded | Real serverdan dinamik olish yoki envga chiqarish |

## Operatsion checklist

1. Yangi test yaratish.
2. Kamida 2 ta variant qo'shish.
3. To'g'ri variantni `true` endpoint orqali belgilash.
4. Test active bo'lganini tekshirish.
5. Rasm bo'lsa 5MB va formatga mosligini tekshirish.
6. User panelda test ko'rinishini sinash.

