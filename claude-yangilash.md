# Darslar — yangilash xatosi: tekshirish va qo'lda yangilash (Maxsumovning Claude'i uchun)

> Claude, bu Valisher tomonidan. Maxsumovda **Settings → Check for updates → Update now** «Turn on Google Apps Script API…» xatosini beryapti, garchi API yoqilgan bo'lsa ham. Platforma 2.3.0 da to'liq ishlayveradi, shoshilinch emas.
>
> Gumon: Google skriptning o'zidan Apps Script API'ni chaqirishga ruxsat bermayapti (bu skript «default» Cloud loyihasida turibdi). Buni aniq bilish uchun quyidagi tekshiruvni Maxsumov bilan qadamma-qadam qil. Har qadamdan keyin skrinshot so'ra.

## 1-qadam. Tekshiruv + yangilash funksiyasini qo'shish
1. Jadvalda **Расширения → Apps Script**.
2. Chapda **Файлы** yonidagi **+** → **Скрипт** → nomi: `Update`.
3. Ichidagi hamma narsani o'chirib, quyidagini qo'ying va **Сохранить** (disk belgisi) ni bosing:

```javascript
function updateDarslarNow() {
  const P = PropertiesService.getScriptProperties();
  const id = P.getProperty('WEBAPP_ID');
  if (!id) throw new Error('WEBAPP_ID topilmadi: Darslar hali o\'rnatilmagan.');
  const base = 'https://valisher111.github.io/darslar/backend/';
  const v = JSON.parse(UrlFetchApp.fetch(base + 'version.json?t=' + Date.now()).getContentText());
  const b = JSON.parse(UrlFetchApp.fetch(base + 'bundle-' + v.build + '.json').getContentText());
  const api = function (m, path, body) {
    const o = { method: m, muteHttpExceptions: true, headers: { Authorization: 'Bearer ' + ScriptApp.getOAuthToken() } };
    if (body) { o.contentType = 'application/json'; o.payload = JSON.stringify(body); }
    const r = UrlFetchApp.fetch('https://script.googleapis.com/v1/projects/' + ScriptApp.getScriptId() + path, o);
    Logger.log(m.toUpperCase() + ' ' + path + ' → ' + r.getResponseCode());
    if (r.getResponseCode() >= 300) throw new Error(r.getContentText().slice(0, 700));
    return JSON.parse(r.getContentText() || '{}');
  };
  api('get', '/deployments');                       // faqat tekshiruv: API ishlaydimi
  api('put', '/content', { files: b.files });       // yangi kod (bu Update fayli o'zi o'chadi)
  const ver = api('post', '/versions', { description: 'Darslar ' + v.build });
  api('put', '/deployments/' + id, { deploymentConfig: { versionNumber: ver.versionNumber, manifestFileName: 'appsscript', description: 'Darslar ' + v.build } });
  Logger.log('TAYYOR: Darslar ' + v.build + ' ga yangilandi.');
}
```

4. Yuqorida funksiya ro'yxatidan **updateDarslarNow** ni tanlang → **▶ Выполнить** (Run). Ruxsat so'ralsa, bering.
5. Pastdagi **Журнал выполнения** (Execution log) ning skrinshotini oling.

## 2-qadam. Natijani o'qish
- **«TAYYOR: Darslar 2.3.1 ga yangilandi»** — hammasi joyida. Saytni yangilang: Settings'da build 2.3.1 ko'rinadi. Keyingi yangilanishlar Settings'dagi tugma bilan ishlashi kerak. Valisherga «yangilandi» deb yozing.
- **«Apps Script API has not been used in project … before or it is disabled»** (yoki `SERVICE_DISABLED`) — gumon tasdiqlandi: Google bu skriptga o'zini yangilashga ruxsat bermaydi. Hech narsa buzilmagan, platforma ishlayveradi. Valisherga **xato matnining skrinshotini** yuboring, u boshqa yangilash yo'lini tanlaydi.
- **«User has not enabled the Apps Script API»** — toggle jadval egasining hisobida yoqilmagan yoki hali kuchga kirmagan. https://script.google.com/home/usersettings ni **maxsumovrustam4@gmail.com** bilan oching, On qiling, 5 daqiqa kutib qayta Run qiling.
- **Boshqa xato** — skrinshotni Valisherga yuboring.

## Eslatmalar
- Bu funksiya jurnal, talabalar, baholar va parolga tegmaydi. U faqat skript kodini almashtiradi.
- Yangilash muvaffaqiyatsiz bo'lsa, eski kod joyida qoladi va platforma o'zgarishsiz ishlayveradi. `Update` faylini o'chirib tashlasa ham bo'ladi.
- 2.3.1 da faqat o'rnatish oynasining matnlari yaxshilangan. Maxsumov uchun bu yangilanish shart emas: maqsad — keyingi yangilanishlar ishlashini tekshirish.

## 3-qadam (23.11 qo'shildi). `SERVICE_DISABLED` chiqdi — Cloud loyihasini ulash
Gumon tasdiqlandi. Valisherning tanlovi: **2-yo'l — Maxsumovning o'z Google Cloud loyihasi.** Jadvalni Valisherga ulashish (1-yo'l) kerak emas.
1. Qo'llanma: https://valisher111.github.io/darslar/help.html#cloud — 8 qadam. Maxsumov bilan qadamma-qadam bajaring, har qadamdan keyin skrinshot so'rang. Hammasi **maxsumovrustam4@gmail.com** hisobida.
2. **Eng muhimi — 4-qadam:** Google Auth Platform → Audience → **Publish app** → holat **In production**. «Testing»da qolsa, 7 kundan keyin ruxsat bekor bo'ladi va `tick` (quizlarni yopish, bot) jim to'xtaydi.
3. 6-qadamdan keyin **Darslar → Install / open Darslar** ni qayta bosib, ruxsat berish shart (Change project avvalgi ruxsatni bekor qiladi).
4. Tekshiruv: saytda Settings → Check for updates → **2.3.2** → Hozir yangilash → «Yangilandi: 2.3.2». `Update` fayli yangilanishda o'zi o'chadi.
5. Natijani (muvaffaqiyat yoki xato skrinshoti) Valisherga yuboring. «Без названия» faylini endi o'chirsa bo'ladi.
