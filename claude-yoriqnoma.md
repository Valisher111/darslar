# Darslar platformasi — Maxsumovning Claude'i uchun yo'riqnoma

> **Claude, bu hujjat senga.** Sen Maxsumov ustozga (ingliz tili o'qituvchisi) Darslar platformasini o'rnatishda va keyin ishlatishda yordam berasan. Platformani V.A. Tangriyev (DTPI) qurgan. Uning Claude'i bu ishni senga topshirdi. Maxsumov bilan **o'zbekcha (lotin)**, qisqa, xuddi inson kabi gaplash. Platforma interfeysi inglizcha, Google Sheets interfeysi esa Maxsumovda **ruscha**. Shuning uchun tugma nomlarini ikkala tilda ayt.
>
> Asosiy qoida: **taxmin qilma.** Har qadamdan keyin Maxsumovdan skrinshot so'ra va ekranda nima turganiga qarab davom et.

---

## 1. Platforma nima

- **Sayt (umumiy, hamma o'qituvchilar uchun bitta):** https://valisher111.github.io/darslar/
  Saytni faqat Valisher yangilaydi. Unga tegilmaydi.
- **Har bir o'qituvchida o'zining Google jadvali va skripti bor.** Jadval — baholar jurnali. Unga bog'langan Apps Script — server. U jadval orqali Apps Script'ning web app (veb-ilova) sifatida nashr qilinadi. Sayt shu web app bilan ishlaydi.
- O'qituvchining sayt havolasi `https://valisher111.github.io/darslar/#api=<DEPLOYMENT_ID>` ko'rinishida bo'ladi. `DEPLOYMENT_ID` — Maxsumov skriptining web app ID'si (`AKfycb…` bilan boshlanadi). Sayt uni bir marta eslab qoladi.
- Talabalar alohida Telegram bot orqali ulanadi (botni Maxsumov o'zi @BotFather'da ochadi).
- Ma'lumotlar (baholar, talabalar) faqat Maxsumovning Google hisobida turadi. Valisher ularni ko'rmaydi.
- To'liq foydalanuvchi qo'llanmasi (o'zbekcha): https://valisher111.github.io/darslar/help.html

### Muhim manzillar
| Nima | Manzil |
|---|---|
| Shablon jadval (nusxa olish uchun) | https://docs.google.com/spreadsheets/d/1635AlY9UzDMx4FAXp02GkCdWb0JaZYz43k7MJVW8HRs/edit |
| Qo'llanma | https://valisher111.github.io/darslar/help.html |
| Skript kodining joriy versiyasi (14 fayl, JSON) | https://valisher111.github.io/darslar/backend/bundle-2.3.1.json |
| Joriy versiya raqami | https://valisher111.github.io/darslar/backend/version.json |
| Quiz skili (Claude uchun) | https://valisher111.github.io/darslar/darslar-quiz.zip |
| Apps Script API yoqish sahifasi | https://script.google.com/home/usersettings |

---

## 2. Hozirgi holat (13.11.2026)

- Maxsumov shablondan nusxa oldi («Darslar-Maxsumov»). Menyudan **Darslar → Install / open Darslar** ni bosdi. Google ruxsati berildi, lekin har safar **«One more step»** oynasi chiqdi. Bu oyna skript o'zini Apps Script API orqali nashr qila olmaganini bildiradi.
- Valisher shablonni 2.3.1 versiyaga yangiladi. Endi oynada aniq sabab va **qo'lda nashr qilish yo'li** ko'rsatiladi, menyuga **Darslar → Connect web app URL** qo'shildi.
- Valisher «yana ishlamadi» dedi. **Qaysi qadamda to'xtagani noma'lum.** Sening birinchi vazifang shuni aniqlash (3-bo'lim).

---

## 3. O'rnatishni tugatish — diagnostika tartibi

### 3.0. Avval so'ra
1. Hozir ekranda nima turibdi? Skrinshot.
2. Jadval qaysi Google hisobida? O'ng yuqoridagi rasm/email.
3. Nusxa qachon olingan? Menyuda **Darslar** ichida nechta band bor?
   - **Ikkita band** bo'lsa («Install / open Darslar» va «Connect web app URL»), bu yangi nusxa (2.3.1).
   - **Bitta band** bo'lsa (faqat Install), bu eski nusxa. Shablondan **qaytadan yangi nusxa** olsin: shablonni ochib → Файл → Создать копию. Eski nusxa kerak emas.

### 3.1. Yo'l A — avtomatik (Apps Script API)
1. https://script.google.com/home/usersettings ni oching. Sahifaning o'ng yuqorisidagi hisob **jadval egasi bilan bir xil** bo'lsin. Brauzerda bir nechta hisob bo'lsa, ko'pincha xato shu yerda bo'ladi: hisobni o'sha sahifaning o'zida almashtiring.
2. «Google Apps Script API» → **On** (Вкл). 3–5 daqiqa kuting.
3. Jadvalda **Darslar → Install / open Darslar**.
4. «Darslar is installed» oynasi chiqsa → **Open Darslar** → 4-bo'limga o'ting.
5. Yana «One more step» chiqsa, oynadagi matnni o'qing:
   - «turn Google Apps Script API to On» deyilsa: toggle hali shu hisobda yoqilmagan yoki kuch hali kirmagan. 1–2-qadamni tekshiring.
   - «Darslar could not publish itself (…)» deyilsa: qavs ichidagi xato matnini yozib oling. Bu holatda **Yo'l B** ga o'ting. Bu Google'ning cheklovi, toggle yordam bermaydi.

### 3.2. Yo'l B — qo'lda nashr qilish (eng ishonchli, 2–3 daqiqa)
Rus interfeysida:
1. Jadvalda: **Расширения → Apps Script**. Muharrir (redaktor) ochiladi.
2. O'ng yuqorida **Начать развертывание** (Deploy) → **Новое развертывание** (New deployment).
3. «Выберите тип» yonidagi tishli g'ildirak → **Веб-приложение** (Web app).
4. Описание: `Darslar`. **Запуск от имени: Я** (Execute as: Me). **У кого есть доступ: Все** (Who has access: Anyone).
   Diqqat: «Все, у кого есть аккаунт Google» emas, aynan **«Все»** bo'lsin.
5. **Начать развертывание / Развернуть** (Deploy). Ruxsat so'ralsa: **Предоставить доступ** → hisob → **Дополнительные настройки** → **Перейти на страницу «Darslar» (небезопасно)** → **Разрешить**.
6. **URL веб-приложения** ni nusxalang. U `https://script.google.com/macros/s/AKfycb…/exec` ko'rinishida bo'lishi shart: oxiri **/exec**, /dev emas.
7. Jadvalga qayting (sahifani yangilang) → **Darslar → Connect web app URL** → URL'ni qo'ying → OK.
8. «Darslar is installed» → **Open Darslar** → 4-bo'lim.

**Tekshirish:** URL'ni brauzerning yangi oynasida oching. «Darslar has moved: https://valisher111.github.io/darslar/» degan oddiy sahifa chiqsa, server ishlayapti.
- «Не удалось открыть файл» yoki Google kirish sahifasi chiqsa, kirish huquqi noto'g'ri. **Управление развертываниями** (Manage deployments) → qalam → «У кого есть доступ: **Все**» → yangi versiya → Развернуть.

### 3.3. Agar «Connect web app URL» ham yordam bermasa
Muharrirda (Расширения → Apps Script) yangi fayl oching (+ → Скрипт), nomi `Link`, ichiga yozing:
```javascript
function showMyLink() {
  const u = 'BU_YERGA_WEB_APP_URL';   // /exec bilan tugaydigan URL
  const id = /\/macros\/s\/([A-Za-z0-9_-]+)\/exec/.exec(u)[1];
  PropertiesService.getScriptProperties().setProperty('WEBAPP_ID', id);
  ensureSheets_(); ensureTrigger_();
  Logger.log(siteLink_(!PropertiesService.getScriptProperties().getProperty('PW_HASH')));
}
```
Funksiya ro'yxatidan `showMyLink` ni tanlab ▶ **Выполнить** (Run) bosing. **Журнал выполнения** (Execution log) da `https://valisher111.github.io/darslar/#api=…&setup=…` havolasi chiqadi. Shu havolani oching va parol qo'ying. Keyin `Link` faylini o'chirib tashlasa bo'ladi.
**Diqqat:** `setup=` li havola birinchi parol qo'yilguncha maxfiy. Uni hech kimga yubormang.

### 3.4. Agar nusxada «Darslar» menyusi umuman chiqmasa
- Sahifani yangilang va 15–20 soniya kuting.
- Расширения → Apps Script ni oching. U yerda `Admin.gs`, `Code.gs`, `Json.gs` … fayllari bormi? Bo'lmasa, nusxaga skript o'tmagan. Shablondan qayta nusxa oling.
- Eng oxirgi chora: yangi bo'sh jadval → Расширения → Apps Script → `bundle-2.3.1.json` dagi 14 faylni bitta-bitta yarating. `name` — fayl nomi (.gs), `source` — matni. `appsscript` — manifest: Настройки проекта → «Показывать файл манифеста appsscript.json» ni yoqing va matnini almashtiring. Keyin Yo'l B. Buni faqat boshqa yo'l qolmaganda qiling. Fayllarni sen (Claude) tayyorlab berasan, Maxsumov joylaydi.

### 3.5. Valisherga nima yuboriladi (o'rnatish tugagach)
Qisqa xabar: qaysi yo'l ishladi (A, B yoki 3.3), «One more step» oynasidagi xato matni (bo'lgan bo'lsa), Settings sahifasidagi «Version … build …». Bu kelajakdagi yangilanishlar uchun kerak.

---

## 4. O'rnatishdan keyin (birinchi sozlash)

1. Saytda **parol** qo'yish (6+ belgi). Saytni xatcho'pga (bookmark) saqlash.
2. **Settings → Your name**: masalan «A.B. Maxsumov».
3. **Groups → + Add group** (har guruh uchun):
   - Guruh nomi (HEMIS'dagidek), fan, dars kunlari, birinchi dars sanasi, 1- va 2-blokdagi darslar soni (odatda 15 + 15).
   - Mustaqil ta'lim: PW1 + PW2 yoki MT1–MT10.
   - Talabalar: HEMIS'dan nusxa, **har qatorda bitta F.I.Sh.**
   - Mavzular ixtiyoriy: har qatorda «Unit 1 - Topic».
   - Jadvalda shu nomli varaq barcha formulalari bilan yaratiladi.
4. **Settings → Student bot**: @BotFather → `/newbot` → nom → `…_bot` username → token → Save. Keyin **Connect my Telegram** → Start.
   Token maxfiy: uni faqat shu maydonga qo'yiladi. Chatda yoki hujjatda saqlanmaydi.
5. Botni guruhning Telegram chatiga qo'shish va u yerga biror xabar yozish. 1 daqiqadan keyin **Groups → Settings → Telegram group chat** dan tanlash.
6. Bot havolasini talabalarga yuborish. Talaba Start → guruh → ism → o'qituvchi tasdiqlaydi.

Darsda ishlatish (qo'llanmada batafsil): **Today**, **Grades** (0 = kelmagan), **Quizzes** (Start as test yoki ▶ Live game), dars oxirida **✓ Finish lesson**.

---

## 5. Quiz tayyorlash (sening asosiy kundalik ishing)

Maxsumov darslik sahifasini beradi (PDF, rasm yoki matn). Sen quizni JSON qilib yozasan. U saytda **Quizzes → Paste from Claude** ga qo'yadi.

Qoidalar:
- Quiz o'tilgan darsning **tilini** tekshiradi: grammatika, so'z boyligi, iboralar. Matndagi faktlarni so'ramaydi.
- Odatiy tarkib: 15 element (**10 MC + 3 TF + 1 gap + 1 match**), 10–15 daqiqa.
- Har elementga qisqa `explain` (qoida, ≤120 belgi) yoziladi.
- MC: 4 variant, bittasi to'g'ri, `answer` — indeks (0 dan). Savol ≤120 belgi, variant ≤75 belgi.

```json
{"title": "Unit 5 - Food and Drink", "unit": "Unit 5", "duration_min": 10,
 "questions": [
  {"id":"q1","type":"mc","prompt":"I'd like ___ water, please.","options":["a","some","any","many"],"answer":1,"explain":"some — in offers and requests"},
  {"id":"q2","type":"tf","prompt":"'Menu' is a countable noun.","answer":true,"explain":"a menu, two menus"},
  {"id":"q3","type":"gap","prompt":"How ___ sugar do you take?","answer":"much","accept":["much"],"explain":"much + uncountable"},
  {"id":"q4","type":"match","prompt":"Match the halves","pairs":[["a cup","of tea"],["a slice","of bread"],["a bottle","of water"],["a bowl","of soup"]],"explain":"containers"},
  {"id":"q5","type":"order","prompt":"Put the words in order","items":["Would","you","like","some","cake?"],"explain":"Would you like + noun?"}
 ]}
```
- Bir nechta quiz kerak bo'lsa, ular `[ {...}, {...} ]` massiv qilib beriladi va hammasi birdan saqlanadi.
- Javob tartibi: bir qator izoh, keyin bitta ```json blok, oxirida «Saytda Quizzes → Paste from Claude ga qo'ying.»
- Batafsil qoidalar `darslar-quiz` skilida (yuqoridagi zip). Maxsumov uni Claude sozlamalaridagi Skills bo'limiga yuklasa, sen uni avtomatik ishlatasan.

---

## 6. Qilinmaydigan ishlar

- Jadvaldagi `_p_` bilan boshlanadigan yashirin varaqlar o'chirilmaydi va qo'lda o'zgartirilmaydi.
- Guruh varag'idagi formula ustunlari (kulrang: Joriy, reyting, MI, ON, Yakuniygacha, Jami, NB soni) qo'lda o'zgartirilmaydi.
- Talaba qatorlari jadvalda qo'lda qo'shilmaydi va o'chirilmaydi. Bu ish saytda qilinadi: **Groups → Students** (Add / Rename / Remove / Restore).
- Dars sanasi saytda ko'chiriladi: **Groups → Lesson dates**.
- Skript kodi o'zgartirilmaydi (3.3 dagi vaqtinchalik funksiyadan tashqari). Xato bo'lsa, Valisherga xabar beriladi.
- Bot tokeni, parol, `setup=` havolasi hech kimga yuborilmaydi.
- Quiz vaqtini uzaytirish kerak bo'lsa, jadvalda emas, saytda **«+2 min»** bilan qilinadi.

---

## 7. Muammo bo'lsa

| Belgi | Nima qilish |
|---|---|
| Sayt «Please sign in again» | Parol bilan qayta kirish. Parol unutilgan bo'lsa → Valisherga. |
| Saytda «Unknown action…» yoki server xatosi | Settings'dagi build raqamini va xato matnini Valisherga yuborish. |
| Bot javob bermaydi | Settings → Student bot «connected»mi? Bot har 1 daqiqada tekshiradi. 00:00–06:00 da xabarlar ertalabgacha kutadi. |
| Baholar rasmi guruhga bormaydi | Groups → Settings → Telegram group chat tanlanganmi? Bot o'sha chatda a'zomi? |
| Skript xatolari | Расширения → Apps Script → chapda **Исполнения** (Executions) → oxirgi qizil qator → matnini Valisherga. |
| Live o'yin ulanmaydi | Internet; QR kod eski bo'lishi mumkin. Ko'p ishlatilsa, o'z Firebase'ini ulash (qo'llanmada «O'z Firebase'ingiz»). |

**Yangilash:** Settings → Gradebook and updates → **Check for updates → Update now**. Agar «Turn on Google Apps Script API» yoki Apps Script API xatosi chiqsa (o'rnatishda Yo'l B ishlatilgan bo'lsa, ehtimoli bor), Valisherga ayting. Yangilashning boshqa yo'li tayyorlanadi. Hozircha yangilanmasa ham platforma ishlayveradi.

---

## 8. Ma'lumot uchun: skript tuzilishi

- Menyu (jadvalda): **Darslar → Install / open Darslar** (`installDarslar`), **Connect web app URL** (`connectWebApp`), **Create TEST sheet**.
- `installDarslar`:
  1. Yashirin `_p_` varaqlarni yaratadi.
  2. Har daqiqalik `tick` triggerini qo'yadi (bot, quiz yopilishi, eslatmalar).
  3. Apps Script API orqali versiya va web app deployment yaratadi va ID'ni `WEBAPP_ID` ga yozadi.
  4. Sayt havolasini ko'rsatadi.
- Jurnal varag'i tuzilishi: 1-qator sarlavha, 2-qator bloklar, 3-qator ustun nomlari (sanalar `dd.mm`), talabalar 4-qatordan. A = №, B = F.I.Sh. Kunlik baho 0–10, 0 = kelmagan.
- Versiya: 2.3.1 (13.11.2026).
