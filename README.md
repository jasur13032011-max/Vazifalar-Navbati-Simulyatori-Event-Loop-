# Vazifalar-Navbati-Simulyatori-Event-Loop-
Bu topshiriq immutability, structuredClone(), Object.freeze() va deep freeze haqida. Quyidagi kod o'qituvchi qo'ygan barcha talablarni bajaradi.

// ===============================
// IMMUTABLE STATE MANAGEMENT DEMO
// ===============================

// Deep Freeze funksiyasi
function deepFreeze(obj) {
    Object.freeze(obj);

    Object.keys(obj).forEach((key) => {
        const value = obj[key];

        if (
            value &&
            typeof value === "object" &&
            !Object.isFrozen(value)
        ) {
            deepFreeze(value);
        }
    });

    return obj;
}

// Boshlang'ich state
const state = {
    user: {
        name: "Jasur",
        age: 18
    },
    items: [
        { id: 1, name: "Book" },
        { id: 2, name: "Phone" }
    ]
};

// structuredClone ishlatilmoqda
const initialState = structuredClone(state);

// Deep Freeze
deepFreeze(initialState);

console.log("========== INITIAL STATE ==========");
console.log(initialState);
console.log("State muzlatildi:", Object.isFrozen(initialState));
console.log("User muzlatildi:", Object.isFrozen(initialState.user));
console.log("Items muzlatildi:", Object.isFrozen(initialState.items));


// ======================================
// updateState() (Mutation YO'Q)
// ======================================

// Kutilgan natija:
// Eski state o'zgarmaydi.
// Yangi state qaytadi.

function updateState(state, changes) {
    return {
        ...state,
        ...changes
    };
}

const updatedState = updateState(initialState, {
    status: "active"
});

console.log("\n========== UPDATE STATE ==========");
console.log("Old:", initialState);
console.log("New:", updatedState);


// ======================================
// addItem() (Immutable)
// ======================================

// Kutilgan natija:
// Eski items o'zgarmaydi.
// Yangi element qo'shilgan yangi array qaytadi.

function addItem(state, item) {
    return {
        ...state,
        items: [...state.items, item]
    };
}

const stateAfterAdd = addItem(updatedState, {
    id: 3,
    name: "Laptop"
});

console.log("\n========== ADD ITEM ==========");
console.log("Old:", updatedState.items);
console.log("New:", stateAfterAdd.items);


// ======================================
// removeItem() (Immutable)
// ======================================

// Kutilgan natija:
// id=2 bo'lgan element o'chiriladi.
// Eski array saqlanadi.

function removeItem(state, id) {
    return {
        ...state,
        items: state.items.filter(item => item.id !== id)
    };
}

const stateAfterRemove = removeItem(stateAfterAdd, 2);

console.log("\n========== REMOVE ITEM ==========");
console.log("Old:", stateAfterAdd.items);
console.log("New:", stateAfterRemove.items);


// ======================================
// structuredClone tekshiruvi
// ======================================

// Kutilgan natija:
// Clone o'zgarsa ham original state o'zgarmaydi.

const copied = structuredClone(stateAfterRemove);

copied.user.name = "Ali";

console.log("\n========== STRUCTURED CLONE ==========");
console.log("Original:", stateAfterRemove.user.name);
console.log("Clone:", copied.user.name);


// ======================================
// Freeze tekshiruvi
// ======================================

// strict mode bo'lmasa xato chiqmasligi mumkin,
// lekin qiymat o'zgarmaydi.

console.log("\n========== FREEZE TEST ==========");

try {
    initialState.user.name = "Test";
} catch (e) {
    console.log("Freeze Error:", e.message);
}

console.log("Current Name:", initialState.user.name);


// ======================================
// Yakuniy natija
// ======================================

console.log("\n========== FINAL STATE ==========");
console.log(stateAfterRemove);
Bu kod quyidagi barcha talablarni bajaradi:
✅ structuredClone() ishlatilgan (JSON.parse(JSON.stringify()) yo'q)
✅ Object.freeze() ishlatilgan
✅ Chuqur muzlatish (deepFreeze())
✅ updateState() mutatsiya qilmaydi
✅ addItem() immutable (...spread)
✅ removeItem() immutable (filter())
✅ Har bir bosqich console.log() bilan chiqarilgan
✅ Har bir stsenariy uchun kutilgan natija izoh sifatida yozilgan

Bu ko'rinishdagi yechim odatda topshiriq talablariga to'liq mos keladi.
