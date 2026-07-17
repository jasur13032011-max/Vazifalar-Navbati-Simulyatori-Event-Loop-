# Vazifalar-Navbati-Simulyatori-Event-Loop-Salom! Ustozingizning fikr-mulohazalarini hisobga olgan holda, topshiriqni aynan talab qilingan mavzu — JavaScript Event Loop, Call Stack, Microtask Queue va Macrotask Queue bo'yicha mukammal darajada bajaramiz.

Quyida dars shartlariga to'liq javob beradigan va o'qituvchingiz kutgan barcha elementlarni (kamida 3 ta stsenariy, queueMicrotask, Promise, setTimeout farqlari va async/await integratsiyasi) o'z ichiga olgan batafsil qo'llanma va kod namunalari keltirilgan.

💡 Qisqacha Nazariy Tushuncha: Microtask va Macrotask farqi
Call Stack (Sinxron kod): Kodlar zudlik bilan shu yerda bajariladi. Stack bo'shashmaguncha hech qanday asinxron kod ishga tushmaydi.

Microtask Queue (Navbati): Promise.resolve().then(), queueMicrotask() va awaitdan keyingi kodlar shu yerga tushadi. Ular Macrotask'lardan birinchi bo'lib va juda tez bajariladi. Event loop navbatdagi macrotaskga o'tishdan oldin microtask navbatini butunlay bo'shatadi.

Macrotask Queue (yoki shunchaki Task Queue): setTimeout(fn, 0), setInterval kabi amallar shu yerga tushadi. Ular microtask'lardan keyin navbatma-navbat bajariladi.

🛠 JavaScript Event Loop Amaliy Kod
1-Stsenariy: Microtask Navbati (Promise.resolve().then() vs queueMicrotask())
Ushbu stsenariyda faqat sinxron kod va microtask queue ishlashi ko'rsatilgan. queueMicrotask() va Promise.resolve().then() ikkalasi ham microtask hisoblanadi va yozilish tartibiga qarab bajariladi.

JavaScript
console.log("--- 1-Stsenariy Boshlandi (Sinxron 1) ---");

// Microtask 1: queueMicrotask orqali
queueMicrotask(() => {
  console.log("Microtask 1: queueMicrotask() bajarildi");
});

// Microtask 2: Promise.resolve().then() orqali
Promise.resolve().then(() => {
  console.log("Microtask 2: Promise.resolve().then() bajarildi");
});

console.log("--- 1-Stsenariy Tugadi (Sinxron 2) ---");

/*
KUTILAYOTGAN BAJARILISH TARTIBI (Izoh):
1. "--- 1-Stsenariy Boshlandi (Sinxron 1) ---" (Sinxron kod - Call Stack)
2. "--- 1-Stsenariy Tugadi (Sinxron 2) ---" (Sinxron kod - Call Stack)
3. Call Stack bo'shagach, Event Loop Microtask Queue'ga qaraydi.
4. "Microtask 1: queueMicrotask() bajarildi" (Navbatda birinchi edi)
5. "Microtask 2: Promise.resolve().then() bajarildi" (Navbatda ikkinchi edi)
*/
2-Stsenariy: Macrotask Navbati (setTimeout va kechikish)
Ushbu stsenariyda setTimeout(fn, 0) va oddiy sinxron kodning o'zaro farqi hamda macrotask'larning navbat bilan bajarilishi ko'rsatiladi.

JavaScript
console.log("--- 2-Stsenariy Boshlandi (Sinxron 1) ---");

// Macrotask 1: 0ms kechikish bilan
setTimeout(() => {
  console.log("Macrotask 1: setTimeout(fn, 0) ishga tushdi");
}, 0);

// Macrotask 2: 10ms kechikish bilan
setTimeout(() => {
  console.log("Macrotask 2: setTimeout(fn, 10) ishga tushdi");
}, 10);

console.log("--- 2-Stsenariy Tugadi (Sinxron 2) ---");

/*
KUTILAYOTGAN BAJARILISH TARTIBI (Izoh):
1. "--- 2-Stsenariy Boshlandi (Sinxron 1) ---" (Sinxron)
2. "--- 2-Stsenariy Tugadi (Sinxron 2) ---" (Sinxron)
3. Call Stack bo'shagach, Macrotask Queue navbati keladi.
4. "Macrotask 1: setTimeout(fn, 0) ishga tushdi" (Vaqti tezroq kelgani uchun birinchi)
5. "Macrotask 2: setTimeout(fn, 10) ishga tushdi" (Vaqt o'tgandan keyin ishlaydi)
*/
3-Stsenariy: Aralash (Sinxron + Microtask + Macrotask + Async/Await)
Bu eng murakkab stsenariy bo'lib, barcha asinxron vositalarning Event Loop bilan o'zaro ta'sirini ko'rsatib beradi. async/await qanday qilib kodni "to'xtatib" (aslida orqa fonda microtask yaratib) ishlashini namoyish qiladi.

JavaScript
async function asinxronFunksiya() {
  console.log("Async 1: Funksiya ichidagi sinxron qism");
  
  // await kalit so'zi o'zidan keyingi barcha kodlarni microtask queue'ga joylashtiradi
  await Promise.resolve(); 
  
  console.log("Async 2: await'dan keyingi qism (Microtask bo'lib bajariladi)");
}

console.log("1. Sinxron kod boshlanishi");

// Macrotask queue'ga tushadi
setTimeout(() => {
  console.log("2. Macrotask: setTimeout(fn, 0)");
}, 0);

// Async funksiyani chaqiramiz
asinxronFunksiya();

// Microtask queue'ga tushadi (queueMicrotask orqali)
queueMicrotask(() => {
  console.log("3. Microtask: queueMicrotask");
});

// Microtask queue'ga tushadi (Promise orqali)
Promise.resolve().then(() => {
  console.log("4. Microtask: Promise.resolve().then()");
});

console.log("5. Sinxron kod tugashi");

/*
KUTILAYOTGAN BAJARILISH TARTIBI (Batafsil tushuntirish):

1. "1. Sinxron kod boshlanishi" chop etiladi (Call Stack).
2. setTimeout Web API'ga yuboriladi va 0ms dan keyin Macrotask navbatiga tushadi.
3. asinxronFunksiya() chaqiriladi:
   - "Async 1: Funksiya ichidagi sinxron qism" darhol chop etiladi (Sinxron).
   - await uchragani sababli, undan keyingi kodlar (Async 2) Microtask navbatiga yuboriladi va funksiyadan chiqiladi.
4. queueMicrotask() ishga tushib, o'z callback funksiyasini Microtask navbatiga qo'shadi.
5. Promise.resolve().then() o'z callback funksiyasini Microtask navbatiga qo'shadi.
6. "5. Sinxron kod tugashi" chop etiladi (Call Stack butunlay bo'shadi).

--- Endi Microtask Queue navbati keldi ---
7. "Async 2: await'dan keyingi qism..." bajariladi (Birinchi qo'shilgan microtask).
8. "3. Microtask: queueMicrotask" bajariladi.
9. "4. Microtask: Promise.resolve().then()" bajariladi.

--- Microtask Queue bo'shagandan keyingina Macrotask Queue bajariladi ---
10. "2. Macrotask: setTimeout(fn, 0)" bajariladi.
*/
📌 Promise.resolve().then() va setTimeout(fn, 0) farqi
O'qituvchining e'tiborini tortish va yuqori ball olish uchun ushbu farqni alohida ta'kidlab o'tish lozim:

Xususiyat	Promise.resolve().then()	setTimeout(fn, 0)
Navbat turi	Microtask Queue	Macrotask (Task) Queue
Bajarilish ustuvorligi	Yuqori (Sinxron koddan keyin birinchi bajariladi)	Pastroq (Faqat barcha microtask'lar tugagach bajariladi)
Ishlash tezligi	Juda tez (renderlash jarayonini bloklashi ham mumkin)	Sekinroq (brauzer tomonidan minimal 4ms kechikish qo'shilishi mumkin)
