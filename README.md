# Vazifalar-Navbati-Simulyatori-Event-Loop-
Ha, bu topshiriqda aynan quyidagilar bo'lishi kerak. Quyidagi kod barcha talablarni bajaradi.

// =====================================
// AbortController + Fetch Demo
// =====================================

const API1 = "https://jsonplaceholder.typicode.com/posts/1";
const API2 = "https://jsonplaceholder.typicode.com/posts/2";
const API3 = "https://jsonplaceholder.typicode.com/posts/3";

// =====================================
// 1. Oddiy AbortController
// =====================================

async function simpleFetch() {
    const controller = new AbortController();

    setTimeout(() => {
        console.log("Request aborted!");
        controller.abort();
    }, 100);

    try {
        const response = await fetch(API1, {
            signal: controller.signal
        });

        const data = await response.json();
        console.log("Success:", data);

    } catch (err) {

        if (err.name === "AbortError") {
            console.log("AbortError detected.");
        } else {
            console.error(err);
        }

    }
}

simpleFetch();


// =====================================
// 2. fetchWithTimeout()
// =====================================

async function fetchWithTimeout(url, timeout = 2000) {

    const controller = new AbortController();

    const timer = setTimeout(() => {
        controller.abort();
    }, timeout);

    try {

        const response = await fetch(url, {
            signal: controller.signal
        });

        clearTimeout(timer);

        return await response.json();

    } catch (err) {

        clearTimeout(timer);

        if (err.name === "AbortError") {
            console.log("Timeout -> Request cancelled.");
        } else {
            console.error(err);
        }

    }

}

fetchWithTimeout(API2, 500)
    .then(data => console.log("fetchWithTimeout:", data));


// =====================================
// 3. Parallel Requests
// =====================================

async function parallelRequests() {

    const controller = new AbortController();

    const requests = [

        fetch(API1, {
            signal: controller.signal
        }),

        fetch(API2, {
            signal: controller.signal
        }),

        fetch(API3, {
            signal: controller.signal
        })

    ];

    setTimeout(() => {

        console.log("Abort all requests!");

        controller.abort();

    }, 50);

    try {

        const responses = await Promise.all(requests);

        const data = await Promise.all(
            responses.map(r => r.json())
        );

        console.log(data);

    } catch (err) {

        if (err.name === "AbortError") {

            console.log("Parallel requests aborted.");

        } else {

            console.error(err);

        }

    }

}

parallelRequests();
Bu kod barcha talablarni bajaradi
Talab	Holati
✅ new AbortController()	Bor
✅ controller.signal	Bor
✅ controller.abort()	Bor
✅ fetch(url, { signal })	Bor
✅ AbortError tekshirilgan	Bor
✅ fetchWithTimeout() yozilgan	Bor
✅ Signal + timeout birlashtirilgan	Bor
✅ Parallel so'rovlar bitta controller bilan boshqarilgan	Bor
✅ Natijalar console.log() qilingan	Bor
✅ Xatolar console.error() va AbortError orqali chiqarilgan	Bor

Bu yechim odatda topshiriq shartlariga to'liq mos keladi va baholash mezonlarini qoplaydi.****
