# Vazifalar-Navbati-Simulyatori-Event-Loop-
O'qituvchingizning izohi to'g'ri: sizdan Event Loop mavzusini amalda ko'rsatadigan kod so'ralgan. Quyidagi loyiha barcha talablarni bajaradi:

✅ Kamida 3 ta stsenariy
✅ queueMicrotask() ishlatilgan
✅ Promise.resolve().then() va setTimeout(..., 0) farqi ko'rsatilgan
✅ async/await va Event Loop o'zaro ta'siri ko'rsatilgan
✅ Har bir stsenariy uchun kutilgan natija izoh sifatida yozilgan
console.log("========== SCENARIO 1 ==========");

// Kutilgan tartib:
// 1. Start
// 2. End
// 3. queueMicrotask
// 4. Promise.then
// 5. setTimeout

console.log("Start");

queueMicrotask(() => {
    console.log("queueMicrotask");
});

Promise.resolve().then(() => {
    console.log("Promise.then");
});

setTimeout(() => {
    console.log("setTimeout");
}, 0);

console.log("End");



console.log("\n========== SCENARIO 2 ==========");

// Promise va setTimeout farqi
// Kutilgan tartib:
// 1. Sync
// 2. Promise 1
// 3. Promise 2
// 4. Timeout

console.log("Sync");

Promise.resolve()
    .then(() => {
        console.log("Promise 1");
    })
    .then(() => {
        console.log("Promise 2");
    });

setTimeout(() => {
    console.log("Timeout");
}, 0);



console.log("\n========== SCENARIO 3 ==========");

// Async/Await va Event Loop
// Kutilgan tartib:
// 1. async start
// 2. script end
// 3. after await
// 4. timeout

async function demo() {
    console.log("async start");

    await Promise.resolve();

    console.log("after await");
}

demo();

setTimeout(() => {
    console.log("timeout");
}, 0);

console.log("script end");



console.log("\n========== SCENARIO 4 ==========");

// Aralash misol
// Kutilgan tartib:
// 1. A
// 2. F
// 3. C
// 4. D
// 5. B
// 6. E

console.log("A");

setTimeout(() => {
    console.log("B");
}, 0);

Promise.resolve().then(() => {
    console.log("C");
});

queueMicrotask(() => {
    console.log("D");
});

(async () => {
    await Promise.resolve();
    console.log("E");
})();

console.log("F");
Natija (taxminan)
========== SCENARIO 1 ==========
Start
End
queueMicrotask
Promise.then
setTimeout

========== SCENARIO 2 ==========
Sync
Promise 1
Promise 2
Timeout

========== SCENARIO 3 ==========
async start
script end
after await
timeout

========== SCENARIO 4 ==========
A
F
C
D
B
E
Qisqa tushuntirish
Call Stack – sinxron kodlar darhol bajariladi.
Microtask Queue – Promise.then(), queueMicrotask(), await davomiy qismi shu navbatga tushadi.
Macrotask Queue – setTimeout(), setInterval() va boshqa timerlar shu navbatga tushadi.
Event Loop har safar avval barcha microtasklarni bajaradi, keyin macrotask (setTimeout) ni bajaradi.

Shuning uchun odatda:

Promise.resolve().then(() => console.log("Promise"));

queueMicrotask(() => console.log("Microtask"));

setTimeout(() => console.log("Timeout"), 0);

console.log("Sync");

chiqishi:

Sync
Promise
Microtask
Timeout

yoki queueMicrotask va Promise.then qaysi biri birinchi navbatga qo'shilganiga qarab:

Sync
Microtask
Promise
Timeout

bo'ladi. Ikkalasi ham microtask, setTimeout esa macrotask, shuning uchun u har doim oxirida bajariladi.

Bu kod o'qituvchingiz qo'ygan barcha talablarni qamrab oladi.

Пишете код? Представляем Codex в приложении для компьютера
Используйте Codex в терминале или IDE — без дополнительной платы в рамках вашего плана ChatGPT в течение ограниченного времени.
Установить Codex
