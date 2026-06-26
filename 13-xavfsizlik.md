# 13. Xavfsizlik

## Mavjud himoya qatlamlari

| Qatlam | Amalga oshirilgan joy | Tavsif |
|--------|------------------------|--------|
| Password hash | Django auth | Parollar Django orqali hash qilinadi |
| Custom token | `UserSession` | UUID token bilan API auth |
| User endpoint guard | `user_required` | Tokenni tekshiradi |
| Admin endpoint guard | `admin_required` | Role va permission tekshiradi |
| Manager permissions | `permissions` JSON | Bo'limlar bo'yicha cheklov |
| Device conflict | `LoginView` | Bir akkaunt boshqa qurilmada bo'lsa 403 |
| Subscription check | `is_expired()` | 30 kunlik muddati tugagan user login qila olmaydi |
| Exam permission | `ruxsat` | Student examdan bloklanishi mumkin |
| Upload validation | Admin test image endpoint | File size va content-type check |
| Media path normalization | `MediaProxyView` | `..` path rad etiladi |

## Auth risklari

| Risk | Hozirgi holat | Tavsiya |
|------|---------------|---------|
| Token localStorageda | XSS bo'lsa token olinishi mumkin | CSP, sanitization, token rotation |
| Token plain DBda | UUID raw saqlanadi | Token hash saqlash |
| Logout revoke emas | `device_info=None`, token qoladi | Session delete yoki revoke timestamp |
| User-Agent device check | User-Agent oson o'zgaradi | Device fingerprintga haddan oshmasdan, session management UI |
| Rate limit yo'q | Login va Telegram code brute force bo'lishi mumkin | Login/code endpointlarga throttling |

## API permission risklari

| Endpoint | Holat | Tavsiya |
|----------|-------|---------|
| `/api/ai/chat/` | Hozir `user_required` yo'q | Auth va `TestSheet.result.user == request.user` check |
| `/api/telegram/generate-code/` | Ochiq/internal | Secret header yoki bot token internal auth |
| `/api/public/connection/` GET | Ochiq | Bu normal, faqat public data qaytaradi |
| `/api/results/` | User decorator bor, admin ham ko'ra oladi | Expected behaviorni dokumentatsiya qilish |

## Settings risklari

| Setting | Hozirgi qiymat | Risk | Tavsiya |
|---------|----------------|------|---------|
| `DEBUG` | `True` | Productionda stack trace va debug leakage | Envdan `False` |
| `SECRET_KEY` | default fallback bor | Env yo'q bo'lsa insecure key | Fallbackni olib tashlash |
| `CORS_ALLOW_ALL_ORIGINS` | `True` | Har qanday origin APIga so'rov yuboradi | Whitelist |
| `ALLOWED_HOSTS` | keng ro'yxat | Keraksiz hostlar qolishi mumkin | Production hostlarni minimallashtirish |
| Bot token | default kodda bor | Secret leak | Env-only |

## File upload xavfsizligi

Mavjud:

- 5 MB limit.
- Content type whitelist.
- Extension whitelist serializerda.
- Eski fayl cleanup.

Qo'shish tavsiya:

1. File header sniffing.
2. Image re-encoding.
3. Virus scan yoki kamida Pillow verify.
4. Random UUID filename.
5. Upload audit log.

## Manager permission modeli

Manager access `panel_perm`ga bog'langan. Misol:

```python
class ThemeView(AdminMavzu):
    panel_perm = "themes"
```

`admin_required`:

- Admin/staff bo'lsa o'tkazadi.
- Manager bo'lsa `permissions` ichida kerakli key borligini tekshiradi.
- `panel_perm=None` bo'lsa manager rad etiladi.
- `panel_perm="*"` bo'lsa har qanday manager o'tadi.

Bu yaxshi model, lekin har bir endpointda `panel_perm` to'g'ri qo'yilganini test qilish kerak.

## Ma'lumot maxfiyligi

| Ma'lumot | Hozirgi ishlatilishi | Tavsiya |
|----------|----------------------|---------|
| `telegram_id` | Userga unique bog'lanadi | Public responsega faqat kerak bo'lsa chiqarish |
| `photo_url` | Profil va rankingda ko'rinadi | User consent va default avatar |
| `ip_address` | UserSessionda saqlanadi | Retention policy |
| `device_info` | Login conflict uchun | Userga ko'rsatishda qisqartiriladi |

## Security checklist

Productionga chiqarishdan oldin:

1. `DEBUG=False`.
2. `SECRET_KEY`, `BOT_TOKEN`, `OPENROUTER_API_KEY` envdan.
3. CORS originlar whitelist.
4. CSRF trusted originlar aniq.
5. HTTPS majburiy.
6. Nginx upload limit 5 MBga mos.
7. `/api/ai/chat/` auth bilan himoyalangan.
8. Login/code rate limit.
9. Admin endpointlar permission testlardan o'tgan.
10. DB backup va restore sinovi bor.

## Tavsiya etilgan testlar

| Test | Kutilgan natija |
|------|-----------------|
| Student admin endpointga kiradi | 403 |
| Manager permission bo'lmagan bo'limga kiradi | 403 |
| Expired student login qiladi | 403 `EXPIRED` |
| `ruxsat=False` student exam boshlaydi | 403 |
| Noto'g'ri token yuboriladi | 401 |
| Boshqa user result_id testsini olish | 400/404 |
| Boshqa user TestSheetga javob | 404 |
| Upload `.exe` image sifatida | 400 |

