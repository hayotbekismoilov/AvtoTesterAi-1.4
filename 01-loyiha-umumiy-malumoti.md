# 1. Loyiha Umumiy Ma'lumoti

## Qisqa tavsif

AvtoTester.uz - haydovchilik guvohnomasi imtihoniga tayyorlanish uchun yaratilgan web va Telegram Mini App platforma. Loyiha foydalanuvchilarga mavzu, bilet, sozlamali test va imtihon rejimida savollar yechish imkonini beradi. Admin panel orqali test bazasi, foydalanuvchilar, natijalar va aloqa ma'lumotlari boshqariladi.

## Asosiy maqsad

| Maqsad | Izoh |
|--------|------|
| Imtihonga tayyorlash | Rasmiy YHQ formatiga yaqin savol-javob jarayoni |
| Bilimni bosqichma-bosqich mustahkamlash | Mavzu va bilet bo'yicha alohida mashq rejimlari |
| Real imtihon muhitini berish | 20 savol, 20 daqiqa, 3 xato bilan tugash qoidasi |
| Natijani kuzatish | Shaxsiy statistika, tarix, top reyting va kunlik reyting |
| Kontentni boshqarish | Admin va manager rollari orqali test, variant, rasm va izohlarni yuritish |

## Auditoriya

| Auditoriya | Platformadagi vazifasi |
|------------|------------------------|
| Talaba yoki o'quvchi | Test ishlaydi, natija va tarixni ko'radi, Telegram akkauntini ulaydi |
| Admin | Tizimni to'liq boshqaradi, foydalanuvchi va kontent yaratadi |
| Manager | Admin bergan aniq bo'limlar doirasida boshqaruv qiladi |
| Dasturchi | Backend, frontend, bot, deploy va xavfsizlikni yuritadi |

## Joriy funksiya to'plami

| Modul | Mavjud imkoniyatlar |
|-------|---------------------|
| Public app | Dashboard, bog'lanish sahifasi, about, login |
| User app | Profil, sozlamalar, statistika, tarix, top reyting, kunlik challenge |
| Test app | Mavzu, bilet, settest va exam rejimlari |
| Test review | Natija sahifasi, to'g'ri javob, izoh, AI yordamchi |
| Admin panel | Users, tests, tickets, themes, results, stats, connections, dashboard |
| Manager panel | Ruxsat berilgan bo'limlar bilan cheklangan admin shell |
| Telegram bot | Mini App tugmasi, yordam, ma'lumot, xato haqida xabar, account linking |
| Media | Test rasmi upload/delete, media proxy, Telegram avatar saqlash |

## Joriy ma'lumotlar hajmi

Lokal SQLite bazasida loyiha aktiv kontentga ega.

| Entity | Soni |
|--------|------|
| User | 666 |
| Theme | 45 |
| Ticket | 61 |
| Test | 1220 |
| Active Test | 1220 |
| Variant | 3991 |
| Result | 81722 |
| TestSheet | 2293947 |

Natija turlari bo'yicha taqsimot:

| Test turi | Natija soni |
|-----------|-------------|
| EXAM | 28049 |
| SETTEST | 14424 |
| THEME | 5189 |
| TICKET | 34060 |

## Muhim biznes qoidalar

| Qoida | Koddagi joyi | Tavsif |
|-------|--------------|--------|
| Obuna muddati | `User.is_expired()` | Student uchun `activated_at + 30 kun`; admin/manager expire bo'lmaydi |
| Imtihon ruxsati | `start_exam` | `ruxsat=False` bo'lsa student exam boshlay olmaydi |
| Exam limiti | `SolveTestViewSet.answer` | 20 daqiqa tugasa yoki 3 xato bo'lsa natija yakunlanadi |
| Test faolligi | `Test.save()` | `correct_answer` bo'lmasa test avtomatik active=False |
| Eski aktiv sessiya | `start_theme/start_ticket/start_settest/start_exam` | Yangi test boshlanganda eski unfinished resultlar yakunlanadi |
| Telegram code | `TelegramCode` API | 6 xonali kod 10 daqiqa ichida ishlaydi |

## Loyiha qiymati

1. Test bazasi real ishlatilayotgan hajmga ega.
2. User flow mobil va Telegram Mini App uchun optimallashtirilgan.
3. Admin panel faqat CRUD emas, balki statistika, server holati va natija review funksiyalarini ham beradi.
4. Manager permissions orqali operatsion ishlarni adminsiz delegatsiya qilish mumkin.
5. AI yordamchi savol kontekstida tushuntirish berishi mumkin.

## Chegaralar

| Chegara | Izoh |
|---------|------|
| Backend DB | Settings hozir SQLite ishlatadi, ammo `psycopg2-binary` mavjudligi Postgresga o'tish rejasini ko'rsatadi |
| Auth token | Custom `UserSession` token ishlatiladi; DRF authtoken o'rnatilgan, lekin asosiy login oqimi custom |
| CORS | `CORS_ALLOW_ALL_ORIGINS=True`; productionda toraytirish kerak |
| AI endpoint | `AiChatView`da hozir `user_required` yo'q; xavfsizlik bo'limida alohida risk sifatida ko'rsatilgan |

## Muvaffaqiyat metrikalari

| Metrika | Nimani bildiradi |
|---------|------------------|
| Exam pass rate | Imtihondan o'tgan foydalanuvchilar ulushi |
| Daily active users | Kunlik test ishlagan foydalanuvchilar |
| Test completion rate | Boshlangan testlarning yakunlangan ulushi |
| Incorrect topics | Eng ko'p xato qilinadigan mavzu va biletlar |
| Support conversion | Bog'lanish/Telegram orqali aktivatsiya so'rovlari |

