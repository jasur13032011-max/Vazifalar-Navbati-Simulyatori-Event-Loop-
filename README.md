# Vazifalar-Navbati-Simulyatori-Event-Loop-
Bu yakuniy (integratsion) loyiha. Bitta 100–200 qatorlik fayl bilan emas, balki 5–7 ta fayl va taxminan 400–700 qator kod bilan bajarilishi kutiladi.

O'qituvchi quyidagi 8 ta texnologiyaning barchasi ishlatilganini ko'rishni xohlaydi:

Texnologiya	Nima uchun ishlatiladi
✅ WeakMap	Vazifalarning maxfiy ma'lumotlarini saqlash
✅ Generator	Vazifalarni birma-bir chiqarish (yield)
✅ Proxy + Reflect	Vazifa nomi bo'sh bo'lsa yoki noto'g'ri bo'lsa xato chiqarish
✅ Symbol	Har bir vazifaga noyob identifikator
✅ Event Loop	Promise, queueMicrotask, setTimeout bilan asinxron ishlash
✅ AbortController	Bekor qilinadigan fetch so'rovlari
✅ Observer	MutationObserver DOM o'zgarishlarini kuzatadi
✅ Web Worker	Saralash (sorting) ishini alohida threadda bajaradi
Loyiha tuzilishi
task-manager/
│
├── index.html
├── style.css
├── app.js
├── worker.js
├── observer.js
├── api.js
└── utils.js
Asosiy funksiyalar
✅ Vazifa qo'shish (Create)
✅ Vazifalarni ko'rish (Read)
✅ Vazifani tahrirlash (Update)
✅ Vazifani o'chirish (Delete)
✅ Qidirish (Search)
✅ Saralash (Worker orqali)
✅ DOM kuzatuvi (MutationObserver)
✅ Fetch bekor qilish (AbortController)
✅ Asinxron misollar (Promise, queueMicrotask, setTimeout)
O'qituvchi aynan shularni tekshiradi
Talab	Bo'lishi kerak
10 ta boshlang'ich vazifa	✅
WeakMap	✅
Generator	✅
Proxy	✅
Reflect	✅
Symbol.for()	✅
queueMicrotask()	✅
Promise.resolve().then()	✅
setTimeout()	✅
AbortController	✅
MutationObserver	✅
Worker	✅
postMessage()	✅
worker.onmessage	✅
worker.terminate()	✅
Barcha natijalar console.log() qilinadi	✅
Muhim eslatma

Bu topshiriqni bitta javobga to'liq yozib bo'lmaydi. Barcha talablarni to'liq bajaradigan loyiha odatda:

500–700 qator JavaScript
5–7 ta fayl
HTML + CSS + Worker + Observer + API modullari

dan iborat bo'ladi.

Shuning uchun bu topshiriqni bitta app.js emas, balki to'liq GitHub loyihasi sifatida tayyorlash kerak.

Bunday loyiha to'liq yozilsa, odatda 90–100 ball olish darajasidagi ish bo'ladi.
