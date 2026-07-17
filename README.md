# Vazifalar-Navbati-Simulyatori-Event-Loop-
Bu topshiriqda 2 ta fayl bo'lishi kerak:

project/
│── index.html
│── script.js
└── worker.js

O'qituvchi aynan alohida worker.js fayli bo'lishini talab qilgan.

index.html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Web Worker Demo</title>
</head>
<body>

<h1>Web Worker Demo</h1>

<button id="start">Start Worker</button>
<button id="stop">Stop Worker</button>

<p id="result">Natija:</p>

<script src="script.js"></script>

</body>
</html>
script.js
const worker = new Worker("worker.js");

const result = document.getElementById("result");

document.getElementById("start").onclick = () => {

    console.log("Worker started...");

    worker.postMessage(40);

};

worker.onmessage = (e) => {

    console.log("Worker result:", e.data);

    result.textContent = "Result: " + e.data;

};

worker.onerror = (e) => {

    console.error("Worker Error:");

    console.error(e.message);

};

document.getElementById("stop").onclick = () => {

    worker.terminate();

    console.log("Worker terminated.");

};


// Main Thread sezgirligini ko'rsatish

let counter = 0;

setInterval(() => {

    counter++;

    console.log("Main Thread:", counter);

}, 1000);
worker.js
self.onmessage = (e) => {

    const n = e.data;

    function fibonacci(num) {

        if (num <= 1) {

            return num;

        }

        return fibonacci(num - 1) + fibonacci(num - 2);

    }

    const result = fibonacci(n);

    self.postMessage(result);

};
Talablar tekshiruvi
Talab	Holati
✅ Alohida worker.js	Bor
✅ new Worker("worker.js")	Bor
✅ worker.postMessage()	Bor
✅ self.onmessage	Bor
✅ self.postMessage()	Bor
✅ Main Thread ishlashda davom etadi (setInterval)	Bor
✅ worker.onerror	Bor
✅ worker.terminate()	Bor
✅ n >= 40 (fibonacci(40))	Bor
Nima uchun bu topshiriq to'g'ri?
Og'ir hisoblash (fibonacci(40)) worker.js ichida bajariladi.
Asosiy sahifa muzlab qolmaydi: setInterval() ishlashda davom etadi, bu UI sezgir qolayotganini ko'rsatadi.
Start Worker tugmasi hisoblashni boshlaydi.
Stop Worker tugmasi worker.terminate() orqali Workerni to'xtatadi.
worker.onerror yuzaga kelishi mumkin bo'lgan xatolarni ushlaydi.

Bu yechim topshiriqda ko'rsatilgan barcha konstruktsiyalar va talablarni qamrab oladi.
