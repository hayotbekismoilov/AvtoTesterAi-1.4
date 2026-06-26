# AvtoTester.uz loyihasi bo'yicha texnik hujjatlar

Ushbu papka AvtoTester.uz loyihasining lokal kod bazasi asosida tayyorlangan to'liq texnik hujjatlar to'plamidir. Hujjatlar backend, frontend, Telegram bot, ma'lumotlar bazasi va joriy loyiha holatini bir joyda tartiblaydi.

## Hujjatlar xaritasi

| # | Fayl | Maqsad |
|---|------|--------|
| 1 | [Loyiha Umumiy Ma'lumoti](01-loyiha-umumiy-malumoti.md) | Loyiha vazifasi, auditoriya, joriy holat va asosiy funksiyalar |
| 2 | [Texnologik Stek](02-texnologik-stek.md) | Frontend, backend, bot va yordamchi kutubxonalar |
| 3 | [Tizim Arxitekturasi](03-tizim-arxitekturasi.md) | Modul chegaralari, data flow va servislar bog'lanishi |
| 4 | [Ma'lumotlar Bazasi Sxemasi](04-malumotlar-bazasi-sxemasi.md) | Django modellari, relationlar va joriy DB statistikasi |
| 5 | [Backend API Spetsifikatsiyasi](05-backend-api-spetsifikatsiyasi.md) | API dizayni, request/response qoidalari va xatoliklar |
| 6 | [Autentifikatsiya va Avtorizatsiya](06-autentifikatsiya-va-avtorizatsiya.md) | Login, token sessiya, role va manager permissions |
| 7 | [Telegram Bot Moduli](07-telegram-bot-moduli.md) | Aiogram bot, Mini App, account linking va komandalar |
| 8 | [Frontend Mobile Web App](08-frontend-mobile-web-app.md) | React ilova, route map, UI oqimlari va Telegram WebApp integratsiyasi |
| 9 | [Admin Panel](09-admin-panel.md) | Admin/manager panel, CRUD, dashboard va statistikalar |
| 10 | [Test Tizimi va Imtihon Logikasi](10-test-tizimi-va-imtihon-logikasi.md) | Test turlari, natija hisoblash, 20 daqiqa va 3 xato qoidasi |
| 11 | [Obuna va Foydalanuvchi Boshqaruvi](11-obuna-va-foydalanuvchi-boshqaruvi.md) | 30 kunlik aktivlik, ruxsat, foydalanuvchi lifecycle |
| 12 | [Fayl va Media Boshqaruvi](12-fayl-va-media-boshqaruvi.md) | Test rasmlari, media proxy, upload limitlari |
| 13 | [Xavfsizlik](13-xavfsizlik.md) | Mavjud himoya qatlamlari, risklar va tavsiyalar |
| 14 | [Deploy va Infrastruktura](14-deploy-va-infrastruktura.md) | Production sozlamalari, Nginx, Gunicorn, bot service |
| 15 | [API Endpoint'lar To'liq Ro'yxati](15-api-endpointlar-toliq-royxati.md) | Barcha endpointlar jadvali |
| 16 | [Loyiha Fayl Strukturasi](16-loyiha-fayl-strukturasi.md) | Kataloglar, asosiy fayllar va mas'uliyatlar |
| 17 | [Kelajakdagi Rivojlantirish Roadmap](17-kelajakdagi-rivojlantirish-roadmap.md) | Texnik qarz, takomillashtirish va roadmap |

## Asosiy manbalar

| Qism | Lokal manba |
|------|-------------|
| Backend sozlamalari | `BackendAvtotester.uz/config/settings.py`, `BackendAvtotester.uz/config/urls.py` |
| API route'lar | `BackendAvtotester.uz/api/urls.py` |
| DB modellari | `BackendAvtotester.uz/api/models.py` |
| API view'lar | `BackendAvtotester.uz/api/views/*.py` |
| Serializerlar | `BackendAvtotester.uz/api/serializers.py` |
| Frontend route'lar | `FrontAvtotester.uz/src/routes/AppRoutes.tsx` |
| Frontend API util | `FrontAvtotester.uz/src/utils/Backend.tsx`, `FrontAvtotester.uz/src/utils/Admin.tsx` |
| Telegram bot | `BackendAvtotester.uz/Bot/*` |

## Joriy loyiha snapshot

Snapshot lokal `BackendAvtotester.uz/db.sqlite3` bazasidan olindi.

| Ko'rsatkich | Qiymat |
|-------------|--------|
| Foydalanuvchilar | 666 |
| Mavzular | 45 |
| Biletlar | 61 |
| Testlar | 1220 |
| Faol testlar | 1220 |
| Variantlar | 3991 |
| Natijalar | 81722 |
| TestSheet yozuvlari | 2293947 |
| Sessiyalar | 528 |

## Hujjat uslubi

- Har bir fayl mustaqil o'qilishi mumkin.
- Jadval va diagrammalar koddagi real nomlarga mos yozilgan.
- Risklar alohida belgilangan, chunki loyiha productionga yaqin ishlatilmoqda.
- Tavsiyalar amaliy va shu kod bazasiga mos qilib berilgan.

