# Vazifalar-Navbati-Simulyatori-Event-Loop-
Bu topshiriqda 3 ta Observer (IntersectionObserver, ResizeObserver, MutationObserver) birgalikda ishlashi kerak. O'qituvchi talab qilgan barcha bandlarni bajaradigan loyiha quyidagi tuzilishda bo'lishi mumkin:

project/
│── index.html
│── style.css
└── script.js
index.html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Observer API Demo</title>
    <link rel="stylesheet" href="style.css">
</head>
<body>

<h1>Observer API Demo</h1>

<div class="spacer"></div>

<div id="target">
    Intersection Target
</div>

<button id="resizeBtn">Resize Box</button>

<div id="resizeBox"></div>

<hr>

<button id="add">Add Item</button>
<button id="remove">Remove Item</button>

<ul id="list"></ul>

<script src="script.js"></script>

</body>
</html>
style.css
body{
    font-family: Arial;
}

.spacer{
    height:900px;
}

#target{
    width:250px;
    height:150px;
    background:orange;
    display:flex;
    justify-content:center;
    align-items:center;
}

#resizeBox{
    width:200px;
    height:120px;
    background:skyblue;
    margin-top:20px;
    transition:.3s;
}
script.js
// ===============================
// IntersectionObserver
// ===============================

const target = document.getElementById("target");

const intersectionObserver = new IntersectionObserver(

(entries)=>{

    entries.forEach(entry=>{

        console.log("Visible:",entry.isIntersecting);

    });

},

{
    threshold:0.5
}

);

intersectionObserver.observe(target);

// 5 soniyadan keyin kuzatishni to'xtatish
setTimeout(()=>{

    intersectionObserver.unobserve(target);

    console.log("Target unobserved.");

},5000);


// ===============================
// ResizeObserver
// ===============================

const box=document.getElementById("resizeBox");

const resizeObserver=new ResizeObserver(entries=>{

    for(const entry of entries){

        console.log(

            "Width:",entry.contentRect.width+"px",

            "Height:",entry.contentRect.height+"px"

        );

    }

});

resizeObserver.observe(box);

document.getElementById("resizeBtn").onclick=()=>{

    box.style.width=(200+Math.random()*200)+"px";

    box.style.height=(120+Math.random()*150)+"px";

};


// ===============================
// MutationObserver
// ===============================

const list=document.getElementById("list");

const mutationObserver=new MutationObserver(mutations=>{

    mutations.forEach(m=>{

        console.log("Mutation:",m.type);

    });

});

mutationObserver.observe(list,{

    childList:true,

    subtree:true

});

let count=1;

document.getElementById("add").onclick=()=>{

    const li=document.createElement("li");

    li.textContent="Item "+count++;

    list.appendChild(li);

    console.log("Item added.");

};

document.getElementById("remove").onclick=()=>{

    if(list.lastElementChild){

        list.removeChild(list.lastElementChild);

        console.log("Item removed.");

    }

};


// ===============================
// Cleanup
// ===============================

window.addEventListener("beforeunload",()=>{

    intersectionObserver.disconnect();

    resizeObserver.disconnect();

    mutationObserver.disconnect();

    console.log("All observers disconnected.");

});
Talablar tekshiruvi
Talab	Holati
✅ IntersectionObserver	Bor
✅ threshold: 0.5	Bor
✅ observe()	Bor
✅ unobserve()	Bor
✅ ResizeObserver	Bor
✅ Pixel (width, height) qiymatlari chiqariladi	Bor
✅ MutationObserver	Bor
✅ childList: true	Bor
✅ subtree: true	Bor
✅ Element qo‘shish kuzatiladi	Bor
✅ Element o‘chirish kuzatiladi	Bor
✅ disconnect()	Bor
✅ console.log() bilan barcha natijalar chiqariladi	Bor

Bu loyiha topshiriqda berilgan barcha konstruktsiyalar (IntersectionObserver, ResizeObserver, MutationObserver, observe, unobserve, disconnect) va baholash mezonlarini to'liq qamrab oladi.
