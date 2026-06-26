# 12. Fayl va Media Boshqaruvi

## Media turlari

| Media turi | Saqlanish joyi | Ishlatilish joyi |
|------------|----------------|------------------|
| Test rasmlari | `BackendAvtotester.uz/media/images/` | Savol ko'rinishida |
| Telegram avatarlar | `BackendAvtotester.uz/media/telegram_avatars/` | User profile/photo_url |
| Frontend public rasmlar | `FrontAvtotester.uz/public/` | `car.png`, `icons.png` |
| Frontend asset | `FrontAvtotester.uz/src/assets/images/` | Dashboard background |

## Django media sozlamasi

```python
MEDIA_URL = "/media/"
MEDIA_ROOT = BASE_DIR / "media"
```

`config/urls.py` development holatida media servis qiladi:

```python
urlpatterns += static(settings.MEDIA_URL, document_root=settings.MEDIA_ROOT)
```

## Test rasmi modeli

`Test.image`:

```python
image = models.ImageField(upload_to="images/", null=True, blank=True)
```

## Rasm upload endpointi

```http
PATCH /api/admin/test/{test_id}/
Authorization: <admin-token>
Content-Type: multipart/form-data
```

Field:

```text
image=<binary file>
```

## Upload validatsiyasi

Endpoint darajasida:

| Cheklov | Qiymat |
|---------|--------|
| Content type | `image/jpeg`, `image/png`, `image/gif`, `image/bmp`, `image/webp` |
| Hajm | 5 MB |

Serializer darajasida:

| Cheklov | Qiymat |
|---------|--------|
| Extension | `jpg`, `jpeg`, `png`, `gif`, `bmp`, `webp` |
| Hajm | 5 MB |

Settings darajasida:

| Setting | Qiymat |
|---------|--------|
| `FILE_UPLOAD_MAX_MEMORY_SIZE` | 50 MB |
| `DATA_UPLOAD_MAX_MEMORY_SIZE` | 50 MB |

Endpoint real validatsiyasi 5 MB bo'lgani uchun amaliy limit 5 MB.

## Rasm almashtirish va o'chirish

`Test.save()`:

- Agar mavjud testda eski rasm bo'lsa va yangi rasm boshqacha bo'lsa, eski fayl diskdan o'chiriladi.
- Agar to'g'ri javob yo'q bo'lsa, test `active=False` bo'ladi.

`Test.delete()`:

- Test o'chirilganda bog'langan rasm fayli ham o'chiriladi.

`PATCH /api/admin/test/{id}/`:

- `image` yuborilmasa, mavjud rasm o'chiriladi va `image=None` qilinadi.

## Media proxy

Admin panel rasmlarni quyidagi endpoint orqali ko'rishi mumkin:

```http
GET /api/admin/media/<path>
```

Himoya:

| Tekshiruv | Natija |
|-----------|--------|
| `..` path ichida bo'lsa | 404 |
| Fayl topilmasa | 404 |
| Token invalid | 401/403 |

Bu Cloudflare yoki static serving muammolarini aylanib o'tish uchun qo'shilgan.

## Frontend image protection

`Solve.tsx`da rasm `<img>` sifatida emas, canvasga chizib ko'rsatiladi.

Bloklanadigan harakatlar:

| Harakat | Blok |
|---------|------|
| Context menu | `preventDefault()` |
| Drag start | `preventDefault()` |
| Mobile long press | CSS `WebkitTouchCallout: none` |
| Direct URL inspect | Canvas sabab DOMda img src ko'rinmaydi |

Bu to'liq xavfsizlik emas, lekin oddiy copy/save harakatlarini qiyinlashtiradi.

## Telegram avatar media

Bot `/start <web_user_id>` oqimida profil rasmni:

```text
../media/telegram_avatars/tg_avatar_<telegram_id>.jpg
```

pathga saqlaydi va `photo_url`ni `/media/telegram_avatars/...` qilib backendga yuboradi.

## Static fayllar

| Qism | Joy |
|------|-----|
| Django static | `STATIC_ROOT = BASE_DIR / "static"` |
| DRF/admin staticfiles | `BackendAvtotester.uz/staticfiles/` |
| Frontend build | `FrontAvtotester.uz/dist/` |
| Frontend public | `FrontAvtotester.uz/public/` |

## Media boshqaruvi risklari

| Risk | Tavsif | Tavsiya |
|------|--------|---------|
| Fayl nomlari | Upload nomlari collision yoki g'alati belgi bo'lishi mumkin | UUID based storage |
| MIME spoofing | Content-Type clientdan keladi | File header sniffing |
| Katta media papka | Git statusda ko'p media o'zgarishlari bor | Media fayllarni gitdan chiqarish |
| Direct media access | `/media/` ochiq bo'lishi mumkin | Kerak bo'lsa signed URL/proxy |
| Disk cleanup | Model delete bor, lekin orphan fayllar qolishi mumkin | Periodik orphan cleanup command |

## Tavsiya etilgan media siyosati

1. Rasm formatini WebPga normalize qilish.
2. Maksimal o'lchamni resize qilish.
3. Fayl nomini `uuid.ext` qilish.
4. Media fayllarni object storagega chiqarish.
5. Admin uploadlar uchun audit log yozish.

