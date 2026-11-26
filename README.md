<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>SPYER — Clothing Brand</title>
<style>
  @import url('https://fonts.googleapis.com/css2?family=Cairo:wght@400;600;700;900&display=swap');

  :root {
    --bg: #0b0b0c;
    --panel: #151618;
    --panel2: #1d1f22;
    --gray: #2d3034;
    --gray-light: #9ea2a8;
    --red: #d62a2a;
    --red-dark: #9f1a1f;
    --white: #f4f4f5;
    --line: #2a2c30;
  }
  * { margin:0; padding:0; box-sizing:border-box; }
  body { font-family:'Cairo',sans-serif; background:var(--bg); color:var(--white); padding-bottom:84px; overflow-x:hidden; }
  body.locked { overflow:hidden; height:100vh; }

  /* ===== Logo splash ===== */
  #intro { position:fixed; inset:0; z-index:200; background:#0b0b0c; display:flex; align-items:center; justify-content:center; transition:opacity .8s ease, visibility .8s; cursor:pointer; }
  #intro.hide { opacity:0; visibility:hidden; }
  #intro img { width:min(86vw,520px); opacity:0; transform:scale(.85); animation:logoIn 1.4s cubic-bezier(.2,.8,.2,1) .2s forwards, glow 1.6s ease-in-out 1.2s 1; -webkit-mask-image:radial-gradient(closest-side,#000 62%,transparent 100%); mask-image:radial-gradient(closest-side,#000 62%,transparent 100%); }
  @keyframes logoIn { to { opacity:1; transform:scale(1); } }
  @keyframes glow { 50% { filter:drop-shadow(0 0 28px rgba(214,42,42,.8)); } }

  /* ===== Hero ===== */
  .hero { position:relative; text-align:center; padding:18px 16px 26px; background:radial-gradient(ellipse at 50% 0%, #2a0d0f 0%, var(--bg) 68%); border-bottom:1px solid var(--line); overflow:hidden; }
  .hero::after { content:""; position:absolute; bottom:0; left:50%; width:140px; height:3px; margin-left:-70px; background:linear-gradient(90deg,transparent,var(--red),transparent); }
  .logo-wrap { width:min(92vw,460px); margin:0 auto; filter:drop-shadow(0 0 34px rgba(214,42,42,.35)); }
  .logo-wrap img { width:100%; display:block; -webkit-mask-image:radial-gradient(closest-side,#000 64%,transparent 100%); mask-image:radial-gradient(closest-side,#000 64%,transparent 100%); }
  .hero .tag { display:inline-block; margin-top:-6px; padding:5px 18px; border:1px solid var(--red); color:var(--red); border-radius:20px; font-size:12px; font-weight:800; letter-spacing:.5px; }
  .hero p { margin-top:10px; font-size:13px; color:var(--gray-light); }

  /* ===== Products ===== */
  .products { max-width:600px; margin:0 auto; padding:22px 14px 20px; display:flex; flex-direction:column; gap:22px; }
  .sec-title { text-align:center; font-size:17px; font-weight:900; color:var(--white); margin-bottom:-6px; }
  .sec-title span { color:var(--red); }
  .card { background:var(--panel); border-radius:20px; padding:12px 12px 16px; border:1.5px solid var(--line); transition:.25s; }
  .card.in-cart { border-color:var(--red); box-shadow:0 0 26px rgba(214,42,42,.2); }
  .img-wrap { width:100%; aspect-ratio:1/1; border-radius:14px; overflow:hidden; background:#000; display:flex; align-items:center; justify-content:center; color:var(--gray-light); font-weight:700; margin-bottom:12px; border:1px solid var(--line); }
  .img-wrap img { width:100%; height:100%; object-fit:cover; }
  .p-name { font-size:18px; font-weight:900; text-align:center; letter-spacing:.3px; direction:ltr; }
  .p-price { font-size:24px; font-weight:900; color:var(--red); text-align:center; margin:4px 0 12px; }

  .qty { display:flex; align-items:center; justify-content:space-between; background:var(--bg); border:1px solid var(--line); border-radius:14px; padding:6px 12px; margin-bottom:10px; }
  .qty button { width:38px; height:38px; border:none; border-radius:10px; background:var(--red); color:#fff; font-size:22px; font-weight:900; cursor:pointer; }
  .qty button:active { transform:scale(.92); }
  .qty span { font-size:18px; font-weight:900; }

  .sizebox { display:none; background:var(--panel2); border:1px solid var(--gray); border-radius:14px; padding:14px; margin-bottom:10px; }
  .card.in-cart .sizebox { display:block; }
  .sizebox .sb-title { font-size:13px; font-weight:800; color:var(--white); margin-bottom:10px; }
  .slide-row { margin-bottom:12px; }
  .slide-head { display:flex; justify-content:space-between; font-size:13px; font-weight:700; color:var(--gray-light); margin-bottom:6px; }
  .slide-head b { color:var(--white); font-size:15px; }
  input[type=range] { width:100%; accent-color:var(--red); height:28px; direction:ltr; }
  .size-result { text-align:center; font-size:12px; color:var(--gray-light); margin-bottom:8px; }
  .size-chips { display:grid; grid-template-columns:repeat(4,1fr); gap:8px; direction:ltr; }
  .chip { padding:10px 0; border:2px solid var(--gray); border-radius:12px; background:var(--bg); color:var(--gray-light); font-family:inherit; font-size:15px; font-weight:900; cursor:pointer; }
  .chip.active { border-color:var(--red); background:var(--red); color:#fff; box-shadow:0 0 14px rgba(214,42,42,.45); }
  .chip:focus-visible, .qty button:focus-visible, .buy:focus-visible, .soc:focus-visible { outline:3px solid #fff; outline-offset:2px; }

  .buy { width:100%; padding:13px; border:none; border-radius:14px; background:linear-gradient(135deg,var(--red),var(--red-dark)); color:#fff; font-family:inherit; font-size:16px; font-weight:900; cursor:pointer; }
  .buy:hover { filter:brightness(1.1); }

  /* ===== Floating bar ===== */
  .floating { position:fixed; bottom:0; right:0; left:0; z-index:50; display:none; align-items:center; justify-content:space-between; padding:12px 16px; background:#111214; border-top:2px solid var(--red); box-shadow:0 -6px 24px rgba(0,0,0,.7); }
  .floating.visible { display:flex; }
  .floating .info { font-size:13px; color:var(--gray-light); }
  .floating .info b { color:var(--red); font-size:16px; }
  .floating button { background:var(--red); color:#fff; border:none; border-radius:12px; padding:10px 20px; font-family:inherit; font-size:14px; font-weight:900; cursor:pointer; }

  /* ===== Order form ===== */
  .order-wrap { max-width:560px; margin:10px auto 30px; padding:0 14px; }
  .order { background:var(--panel); padding:24px 18px; border-radius:20px; border:1px solid var(--line); border-top:4px solid var(--red); box-shadow:0 14px 36px rgba(0,0,0,.5); }
  .order h3 { text-align:center; font-size:19px; font-weight:900; margin-bottom:16px; }
  .fg { margin-bottom:12px; }
  .fg label { display:block; font-size:13px; font-weight:700; color:var(--gray-light); margin-bottom:4px; }
  .fg input, .fg select { width:100%; padding:11px 12px; border:1.5px solid var(--gray); border-radius:12px; font-family:inherit; font-size:14px; background:var(--bg); color:var(--white); outline:none; }
  .fg input:focus, .fg select:focus { border-color:var(--red); }
  .fg input::placeholder { color:#6b6f76; }
  .cart-box { background:var(--bg); border:1px dashed var(--gray); border-radius:14px; padding:12px; margin-bottom:14px; }
  .cart-box .t { font-size:12px; font-weight:800; color:var(--red); margin-bottom:6px; }
  .line { display:flex; justify-content:space-between; gap:10px; font-size:13px; padding:6px 0; border-bottom:1px solid var(--line); }
  .line:last-child { border-bottom:none; }
  .line small { display:block; color:var(--gray-light); font-size:11px; margin-top:2px; }
  .empty { text-align:center; font-size:12px; color:var(--gray-light); }

  .del-grid { display:grid; grid-template-columns:1fr 1fr; gap:10px; margin-bottom:14px; }
  .del { border:2px solid var(--gray); border-radius:14px; padding:10px; text-align:center; cursor:pointer; background:var(--bg); transition:.2s; }
  .del.active { border-color:var(--red); background:rgba(214,42,42,.12); }
  .del.disabled-opt { opacity:.4; cursor:not-allowed; pointer-events:none; }
  .del .dt { font-size:12px; font-weight:700; }
  .del .dp { font-size:13px; font-weight:900; color:var(--red); margin-top:2px; }

  .summary { background:var(--bg); border:1px solid var(--line); border-radius:14px; padding:14px; margin:6px 0 16px; }
  .row { display:flex; justify-content:space-between; font-size:13px; padding:3px 0; color:var(--gray-light); }
  .row.total { font-size:18px; font-weight:900; color:var(--white); border-top:1px solid var(--line); margin-top:5px; padding-top:7px; }
  .row.total span:last-child { color:var(--red); }
  .submit { width:100%; padding:15px; border:none; border-radius:14px; background:linear-gradient(135deg,var(--red),var(--red-dark)); color:#fff; font-family:inherit; font-size:17px; font-weight:900; cursor:pointer; box-shadow:0 6px 22px rgba(214,42,42,.35); }
  .submit:disabled { opacity:.45; cursor:not-allowed; box-shadow:none; }
  .hint { font-size:12px; color:#ff8a8a; text-align:center; margin-top:8px; min-height:16px; }

  .faq { margin-top:20px; border-top:2px dashed var(--gray); padding-top:14px; }
  .faq h4 { text-align:center; font-size:15px; font-weight:900; margin-bottom:10px; }
  .faq-item { background:var(--bg); border:1px solid var(--line); border-radius:12px; padding:10px 14px; margin-bottom:8px; }
  .faq-item b { font-size:12px; display:block; }
  .faq-item span { font-size:12px; font-weight:600; color:var(--gray-light); }

  .social { text-align:center; padding:10px 14px 36px; }
  .social h4 { font-size:15px; font-weight:900; margin-bottom:14px; }
  .social-btns { display:flex; justify-content:center; gap:12px; flex-wrap:wrap; }
  .soc { display:flex; align-items:center; gap:8px; padding:11px 18px; border-radius:14px; background:var(--panel2); border:1px solid var(--gray); color:#fff; text-decoration:none; font-size:14px; font-weight:800; transition:.2s; }
  .soc svg { width:20px; height:20px; fill:currentColor; }
  .soc:hover { transform:translateY(-2px); background:var(--red); border-color:var(--red); }

  @media (prefers-reduced-motion: reduce) { #intro img { animation-duration:.01s; animation-delay:0s; } }
</style>
</head>
<body class="locked">

<div id="intro" onclick="closeIntro()"><img src="logo.jpg" alt="SPYER"></div>

<div class="hero">
  <div class="logo-wrap"><img src="logo.jpg" alt="SPYER Clothing Brand"></div>
  <span class="tag">🕷️ NEW COLLECTION — HOODIES</span>
  <p>هوديز بتصميم قوي وجودة عالية — اختر مقاسك بدقة</p>
</div>

<div class="products">
  <div class="sec-title">اختر <span>الهودي</span> الخاص بك</div>
  <div id="products" style="display:flex;flex-direction:column;gap:22px;"></div>
</div>

<div class="floating" id="floating">
  <div class="info">المنتجات: <b id="fc-count">0</b> — المجموع: <b id="fc-total">0 دج</b></div>
  <button onclick="scrollToOrder()">إتمام الطلب ←</button>
</div>

<div class="order-wrap" id="order-section">
  <div class="order">
    <h3>🛒 معلومات الطلب والتوصيل</h3>

    <div class="cart-box">
      <div class="t">🧾 المنتجات المختارة</div>
      <div id="cart-lines"><div class="empty">لم تختر أي منتج بعد</div></div>
    </div>

    <div class="fg"><label>الاسم واللقب</label><input type="text" id="fullname" placeholder="أدخل اسمك الكامل"></div>
    <div class="fg"><label>رقم الهاتف</label><input type="tel" id="phone" placeholder="06XXXXXXXX"></div>
    <div class="fg">
      <label>الولاية</label>
      <select id="wilaya" onchange="updateDelivery()"><option value="">— اختر الولاية —</option></select>
    </div>

    <label style="font-size:13px;font-weight:700;color:var(--gray-light);display:block;margin-bottom:6px;">نوع التوصيل</label>
    <div class="del-grid">
      <div class="del" id="btn-home" onclick="setDelivery('home')"><div class="dt">🏠 للمنزل</div><div class="dp" id="p-home">—</div></div>
      <div class="del" id="btn-office" onclick="setDelivery('office')"><div class="dt" id="t-office">🏢 للمكتب</div><div class="dp" id="p-office">—</div></div>
    </div>

    <div class="fg"><label>البلدية / العنوان</label><input type="text" id="address" placeholder="اسم البلدية أو العنوان"></div>

    <div class="summary">
      <div class="row"><span>مجموع المنتجات:</span><span id="sum-products">0 دج</span></div>
      <div class="row"><span>سعر التوصيل:</span><span id="sum-del">—</span></div>
      <div class="row total"><span>المجموع الكلي:</span><span id="sum-total">—</span></div>
    </div>

    <button class="submit" id="submit-btn" onclick="sendOrder()" disabled>تأكيد الطلب الآن</button>
    <div class="hint" id="hint"></div>

    <div class="faq">
      <h4>❓ أسئلة شائعة</h4>
      <div class="faq-item"><b>💳 طريقة الدفع؟</b><span>الدفع يد بيد عند استلام طلبيتك وإعادة معاينتها.</span></div>
      <div class="faq-item"><b>🚚 مدة التوصيل؟</b><span>التوصيل سريع من يومين إلى ثلاثة أيام كأقصى حد.</span></div>
    </div>
  </div>
</div>

<div class="social">
  <h4>تابعنا على مواقع التواصل 🕷️</h4>
  <div class="social-btns">
    <a class="soc" href="[https://www.instagram.com/luna__pack?xtok=MzF1ZDh3cGk5bWt0](https://www.instagram.com/spyer_fit?evlk=N3BhMzM3MnY4amZk)" target="_blank" rel="noopener noreferrer">
      <svg viewBox="0 0 24 24" aria-hidden="true"><path d="M7.5 2h9A5.5 5.5 0 0 1 22 7.5v9a5.5 5.5 0 0 1-5.5 5.5h-9A5.5 5.5 0 0 1 2 16.5v-9A5.5 5.5 0 0 1 7.5 2zm0 2A3.5 3.5 0 0 0 4 7.5v9A3.5 3.5 0 0 0 7.5 20h9a3.5 3.5 0 0 0 3.5-3.5v-9A3.5 3.5 0 0 0 16.5 4h-9zM12 7a5 5 0 1 1 0 10 5 5 0 0 1 0-10zm0 2a3 3 0 1 0 0 6 3 3 0 0 0 0-6zm5.3-3.1a1.2 1.2 0 1 1 0 2.4 1.2 1.2 0 0 1 0-2.4z"/></svg>
      انستغرام
    </a>
    <a class="soc" href="[https://www.facebook.com/share/1K33qXmHeA/](https://www.instagram.com/spyer_fit?evlk=N3BhMzM3MnY4amZk)" target="_blank" rel="noopener noreferrer">
      <svg viewBox="0 0 24 24" aria-hidden="true"><path d="M13.5 21v-8h2.7l.4-3.2h-3.1V7.9c0-.9.3-1.5 1.6-1.5h1.6V3.5c-.3 0-1.3-.1-2.4-.1-2.4 0-4 1.5-4 4.1v2.3H7.6V13h2.7v8h3.2z"/></svg>
      فيسبوك
    </a>
    <a class="soc" href="[https://www.tiktok.com/@luna___pack?_r=1&amp;_t=ZS-9ApjTwB0iwS](https://tiktok.com/@spyer_fit)" target="_blank" rel="noopener noreferrer">
      <svg viewBox="0 0 24 24" aria-hidden="true"><path d="M16.6 3h-3.2v12.2a2.7 2.7 0 1 1-2.7-2.7c.3 0 .5 0 .8.1V9.3a5.9 5.9 0 1 0 5.1 5.9V8.6a7.4 7.4 0 0 0 4.3 1.4V6.8A4.3 4.3 0 0 1 16.6 3z"/></svg>
      تيك توك
    </a>
  </div>
</div>

<script>
var TELEGRAM_TOKEN = "8984328868:AAEjxhYfk_Iw6PhnSEIrsTZ3zCd_7zZHiLA";
var TELEGRAM_CHAT_ID = "8765345419";

var RATES = {
  "01 - Adrar": [1500, 750], "02 - Chlef": [900, 450], "03 - Laghouat": [1000, 500],
  "04 - Oum El Bouaghi": [750, 450], "05 - Batna": [450, 300], "06 - Bejaia": [800, 450],
  "07 - Biskra": [750, 450], "08 - Bechar": [1000, 500], "09 - Blida": [700, 450],
  "10 - Bouira": [900, 450], "11 - Tamanrasset": [1700, 800], "12 - Tebessa": [900, 450],
  "13 - Tlemcen": [950, 450], "14 - Tiaret": [950, 450], "15 - Tizi Ouzou": [700, 450],
  "16 - Alger": [700, 450], "17 - Djelfa": [1050, 600], "18 - Jijel": [800, 450],
  "19 - Setif": [700, 450], "20 - Saida": [900, 500], "21 - Skikda": [750, 450],
  "22 - Sidi Bel Abbes": [800, 0], "23 - Annaba": [750, 450], "24 - Guelma": [750, 450],
  "25 - Constantine": [700, 450], "26 - Medea": [800, 450], "27 - Mostaganem": [800, 450],
  "28 - Msila": [800, 450], "29 - Mascara": [800, 450], "30 - Ouargla": [900, 450],
  "31 - Oran": [800, 450], "32 - El Bayadh": [1150, 600], "33 - Illizi": [1700, 750],
  "34 - Bordj Bou Arreridj": [700, 450], "35 - Boumerdes": [700, 450], "36 - El Tarf": [750, 450],
  "37 - Tindouf": [1700, 0], "38 - Tissemsilt": [800, 450], "39 - El Oued": [900, 500],
  "40 - Khenchela": [600, 450], "41 - Souk Ahras": [700, 450], "42 - Tipaza": [850, 450],
  "43 - Mila": [700, 450], "44 - Ain Defla": [800, 450], "45 - Naama": [1050, 500],
  "46 - Ain Temouchent": [800, 450], "47 - Ghardaia": [950, 600], "48 - Relizane": [800, 500],
  "49 - Timimoun": [1400, 0], "50 - Bordj Badji Mokhtar": [1500, 0], "51 - Ouled Djellal": [950, 500],
  "52 - Beni Abbes": [1000, 0], "53 - In Salah": [1500, 750], "54 - In Guezzam": [1550, 0],
  "55 - Touggourt": [900, 500], "56 - Djanet": [1600, 750], "57 - El Mghair": [900, 500],
  "58 - El Meniaa": [1000, 0]
};

var PRODUCTS = [
  { id: 1, name: "hoodie black and white", price: 4000, img: "صورة1.Png" },
  { id: 2, name: "hoodie SPYER", price: 4300, img: "صورة2.png" }
];
var SIZES = ["S", "M", "L", "XL"];

var cart = {};   // id -> qty
var sel = {};    // id -> {h, w, size, ready}
var currentDeliveryType = null;

function closeIntro() {
  var el = document.getElementById('intro');
  if (!el || el.classList.contains('hide')) return;
  el.classList.add('hide');
  document.body.classList.remove('locked');
  setTimeout(function(){ el.remove(); }, 900);
}
setTimeout(closeIntro, 2800);

/* مقترح المقاس حسب الطول والوزن */
function calcSize(h, w) {
  var score = w + (h - 170) * 0.5;
  if (score < 60) return "S";
  if (score < 75) return "M";
  if (score < 90) return "L";
  return "XL";
}
function taille(s) { return (s === "S" || s === "M") ? "taille1" : "taille2"; }

function renderProducts() {
  var html = '';
  PRODUCTS.forEach(function(p) {
    sel[p.id] = { h: 170, w: 70, size: calcSize(170, 70), ready: false };
    var chips = SIZES.map(function(s) {
      return '<button type="button" class="chip' + (s === sel[p.id].size ? ' active' : '') + '" data-s="' + s + '" onclick="chooseSize(' + p.id + ',\'' + s + '\')">' + s + '</button>';
    }).join('');
    html += '<div class="card" id="card-' + p.id + '">' +
      '<div class="img-wrap"><img src="' + p.img + '" alt="' + p.name + '" onerror="this.parentElement.innerHTML=\'صورة المنتج\'"></div>' +
      '<div class="p-name">' + p.name + '</div>' +
      '<div class="p-price">' + p.price + ' دج</div>' +
      '<div class="qty"><button onclick="changeQty(' + p.id + ',-1)" aria-label="إنقاص">−</button><span id="qty-' + p.id + '">0</span><button onclick="changeQty(' + p.id + ',1)" aria-label="زيادة">+</button></div>' +
      '<div class="sizebox" id="sizebox-' + p.id + '">' +
        '<div class="sb-title">📏 اعرف مقاسك المناسب</div>' +
        '<div class="slide-row"><div class="slide-head"><span>الطول</span><b><span id="hv-' + p.id + '">170</span> سم</b></div>' +
          '<input type="range" min="140" max="210" value="170" oninput="onSlide(' + p.id + ')" id="h-' + p.id + '" aria-label="الطول"></div>' +
        '<div class="slide-row"><div class="slide-head"><span>الوزن</span><b><span id="wv-' + p.id + '">70</span> كغ</b></div>' +
          '<input type="range" min="35" max="150" value="70" oninput="onSlide(' + p.id + ')" id="w-' + p.id + '" aria-label="الوزن"></div>' +
        '<div class="size-result" id="sr-' + p.id + '">حرّك الشريطين لمعرفة المقاس المناسب لك</div>' +
        '<div class="size-chips" id="chips-' + p.id + '">' + chips + '</div>' +
      '</div>' +
      '<button class="buy" onclick="buyNow(' + p.id + ')">⚡ اشتري الآن</button>' +
    '</div>';
  });
  document.getElementById('products').innerHTML = html;
}

function paintChips(id) {
  document.querySelectorAll('#chips-' + id + ' .chip').forEach(function(c) {
    c.classList.toggle('active', c.getAttribute('data-s') === sel[id].size);
  });
}

function onSlide(id) {
  var h = +document.getElementById('h-' + id).value;
  var w = +document.getElementById('w-' + id).value;
  sel[id].h = h; sel[id].w = w; sel[id].ready = true;
  sel[id].size = calcSize(h, w);
  document.getElementById('hv-' + id).innerText = h;
  document.getElementById('wv-' + id).innerText = w;
  document.getElementById('sr-' + id).innerHTML = 'المقاس المناسب لك: <b style="color:#d62a2a;font-size:16px;">' + sel[id].size + '</b>';
  paintChips(id);
  updateCartUI();
}

function chooseSize(id, s) {
  sel[id].size = s; sel[id].ready = true;
  document.getElementById('sr-' + id).innerHTML = 'المقاس المختار: <b style="color:#d62a2a;font-size:16px;">' + s + '</b>';
  paintChips(id);
  updateCartUI();
}

function setQty(id, n) {
  cart[id] = n;
  document.getElementById('qty-' + id).innerText = n;
  document.getElementById('card-' + id).classList.toggle('in-cart', n > 0);
  updateCartUI();
}
function changeQty(id, d) { setQty(id, Math.max(0, (cart[id] || 0) + d)); }

function buyNow(id) {
  if (!cart[id]) setQty(id, 1);
  if (!sel[id].ready) {
    document.getElementById('sizebox-' + id).scrollIntoView({ behavior: 'smooth', block: 'center' });
    return;
  }
  scrollToOrder();
}

function getCartItems() {
  var items = [];
  PRODUCTS.forEach(function(p) {
    var q = cart[p.id] || 0;
    if (q > 0) items.push({ p: p, qty: q, lineTotal: q * p.price, s: sel[p.id] });
  });
  return items;
}
function getSubtotal() { return getCartItems().reduce(function(a, i) { return a + i.lineTotal; }, 0); }
function allSizesReady() { return getCartItems().every(function(i) { return i.s.ready; }); }

function updateCartUI() {
  var items = getCartItems();
  var count = items.reduce(function(a, i) { return a + i.qty; }, 0);
  var sub = getSubtotal();
  document.getElementById('fc-count').innerText = count;
  document.getElementById('fc-total').innerText = sub + ' دج';
  document.getElementById('floating').classList.toggle('visible', count > 0);

  var html = '';
  if (!items.length) html = '<div class="empty">لم تختر أي منتج بعد</div>';
  items.forEach(function(i) {
    var info = i.s.ready ? 'المقاس: ' + i.s.size + ' — الطول ' + i.s.h + ' سم — الوزن ' + i.s.w + ' كغ' : 'لم يتم تحديد المقاس بعد';
    html += '<div class="line"><span><span dir="ltr">' + i.p.name + '</span> × ' + i.qty + '<small>' + info + '</small></span><span>' + i.lineTotal + ' دج</span></div>';
  });
  document.getElementById('cart-lines').innerHTML = html;
  document.getElementById('sum-products').innerText = sub + ' دج';
  calculateTotal();
}

function scrollToOrder() { document.getElementById('order-section').scrollIntoView({ behavior: 'smooth', block: 'start' }); }

document.addEventListener('DOMContentLoaded', function() {
  renderProducts();
  var s = document.getElementById('wilaya');
  for (var w in RATES) {
    var o = document.createElement('option');
    o.value = w; o.textContent = w;
    s.appendChild(o);
  }
});

function updateDelivery() {
  var w = document.getElementById('wilaya').value;
  var btnOffice = document.getElementById('btn-office');
  if (!w || !RATES[w]) return;
  var home = RATES[w][0], office = RATES[w][1];
  document.getElementById('p-home').innerText = home ? home + ' دج' : 'غير متوفر';
  if (office === 0) {
    btnOffice.classList.add('disabled-opt');
    btnOffice.classList.remove('active');
    document.getElementById('t-office').innerText = '🏢 لا يوجد مكتب في ولايتك';
    document.getElementById('p-office').innerText = 'غير متوفر';
    if (currentDeliveryType === 'office') {
      currentDeliveryType = 'home';
      document.getElementById('btn-home').classList.add('active');
    }
  } else {
    btnOffice.classList.remove('disabled-opt');
    document.getElementById('t-office').innerText = '🏢 للمكتب';
    document.getElementById('p-office').innerText = office + ' دج';
  }
  calculateTotal();
}

function setDelivery(type) {
  var w = document.getElementById('wilaya').value;
  if (!w || !RATES[w]) return;
  if (type === 'office' && RATES[w][1] === 0) return;
  currentDeliveryType = type;
  document.getElementById('btn-home').classList.toggle('active', type === 'home');
  document.getElementById('btn-office').classList.toggle('active', type === 'office');
  calculateTotal();
}

function calculateTotal() {
  var w = document.getElementById('wilaya').value;
  var sub = getSubtotal();
  var hint = document.getElementById('hint');
  var btn = document.getElementById('submit-btn');
  hint.innerText = '';

  if (sub === 0 || !w || !currentDeliveryType || !RATES[w]) {
    document.getElementById('sum-del').innerText = '—';
    document.getElementById('sum-total').innerText = '—';
    btn.disabled = true;
    return;
  }
  var cost = currentDeliveryType === 'home' ? RATES[w][0] : RATES[w][1];
  document.getElementById('sum-del').innerText = cost + ' دج';
  document.getElementById('sum-total').innerText = (sub + cost) + ' دج';

  if (!allSizesReady()) {
    hint.innerText = 'يرجى تحديد الطول والوزن (أو اختيار المقاس) تحت كل هودي.';
    btn.disabled = true;
    return;
  }
  btn.disabled = false;
}

function sendOrder() {
  var name = document.getElementById('fullname').value.trim();
  var phone = document.getElementById('phone').value.trim();
  var wilaya = document.getElementById('wilaya').value;
  var address = document.getElementById('address').value.trim();
  var items = getCartItems();
  var sub = getSubtotal();

  if (!name || !phone) { alert('يرجى كتابة الاسم ورقم الهاتف'); return; }
  if (!items.length) { alert('يرجى اختيار منتج واحد على الأقل'); return; }
  if (!allSizesReady()) { alert('يرجى تحديد المقاس'); return; }

  var deliveryText = currentDeliveryType === 'home' ? 'توصيل للمنزل' : 'توصيل للمكتب';
  var cost = currentDeliveryType === 'home' ? RATES[wilaya][0] : RATES[wilaya][1];
  var total = sub + cost;

  var btn = document.getElementById('submit-btn');
  btn.disabled = true;
  btn.innerText = 'جاري إرسال الطلب...';

  var itemsText = items.map(function(i) {
    return '• ' + i.p.name + ' × ' + i.qty + ' = ' + i.lineTotal + ' دج\n' +
           '   📏 المقاس: ' + taille(i.s.size) + ' (الطول ' + i.s.h + ' سم، الوزن ' + i.s.w + ' كغ)';
  }).join('\n');

  var message = '🛒 طلبية جديدة — SPYER\n\n' +
    '👤 الاسم: ' + name + '\n' +
    '📞 الهاتف: ' + phone + '\n' +
    '📍 الولاية: ' + wilaya + '\n' +
    '🏠 العنوان/البلدية: ' + (address || 'غير محدد') + '\n\n' +
    '📦 المنتجات:\n' + itemsText + '\n\n' +
    '🚚 نوع التوصيل: ' + deliveryText + ' (' + cost + ' دج)\n' +
    '💰 مجموع المنتجات: ' + sub + ' دج\n' +
    '💰 المجموع الكلي: ' + total + ' دج';

  fetch('https://api.telegram.org/bot' + TELEGRAM_TOKEN + '/sendMessage', {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify({ chat_id: TELEGRAM_CHAT_ID, text: message })
  })
  .then(function(r) { return r.json(); })
  .then(function(d) {
    if (d.ok) {
      alert('تم تسجيل طلبك بنجاح! وسنتصل بك قريباً لتأكيده.');
      location.reload();
    } else {
      alert('حدث خطأ أثناء إرسال الطلب، يرجى المحاولة لاحقاً.');
      btn.disabled = false; btn.innerText = 'تأكيد الطلب الآن';
    }
  })
  .catch(function() {
    alert('تعذر الاتصال بالخادم، تحقق من الاتصال بالإنترنت.');
    btn.disabled = false; btn.innerText = 'تأكيد الطلب الآن';
  });
}
</script>
</body>
</html>
