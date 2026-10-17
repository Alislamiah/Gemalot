<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Gemalot</title>
<script src="https://cdnjs.cloudflare.com/ajax/libs/mathjs/11.11.0/math.min.js"></script>
<style>
/* ===== Splash - ستايلك الأصلي مع icon.png ===== */
.ai-splash-screen{position:fixed;inset:0;background:#07111f;display:flex;flex-direction:column;justify-content:center;align-items:center;z-index:99999;overflow:hidden;perspective:1000px;transition:opacity .8s cubic-bezier(0.4,0,0.2,1),visibility .8s ease}
.ai-splash-screen.hidden{opacity:0;visibility:hidden;pointer-events:none}
.ai-splash-glow{position:absolute;width:280px;height:280px;background:radial-gradient(circle,rgba(8,127,255,.35) 0%,rgba(0,195,255,.12) 50%,transparent 75%);border-radius:50%;animation:bgGlow 3s ease-in-out infinite alternate}
.ai-icon-container{position:relative;z-index:2;display:flex;justify-content:center;align-items:center;transform-style:preserve-3d;animation:flipAndScale 2s cubic-bezier(0.34,1.56,0.64,1) forwards}
.ai-splash-icon{width:110px;height:110px;border-radius:26px;object-fit:cover;box-shadow:0 12px 35px rgba(8,127,255,.45),0 0 20px rgba(0,195,255,.25);animation:floatingGlow 2.5s ease-in-out 2s infinite alternate}
.ai-splash-text{position:relative;z-index:2;margin-top:30px;font-size:1.3rem;font-weight:700;color:#f0f7ff;opacity:0;transform:translateY(18px);animation:textAppear .8s ease-out 1.5s forwards;text-shadow:0 4px 18px rgba(0,0,0,.7)}
@keyframes flipAndScale{0%{opacity:0;transform:scale(.2) rotateY(180deg) rotateX(45deg) translateY(100px)}50%{opacity:1;transform:scale(1.25) rotateY(360deg) rotateX(-15deg) translateY(-15px)}75%{transform:scale(.95) rotateY(540deg) rotateX(5deg) translateY(5px)}100%{opacity:1;transform:scale(1) rotateY(720deg) rotateX(0) translateY(0)}}
@keyframes floatingGlow{0%{transform:scale(1)}100%{transform:scale(1.05);box-shadow:0 18px 45px rgba(8,127,255,.65),0 0 30px rgba(0,195,255,.5)}}
@keyframes bgGlow{0%{transform:scale(.85);opacity:.5}100%{transform:scale(1.2);opacity:.9}}
@keyframes textAppear{to{opacity:1;transform:translateY(0)}}

/* ===== App - مطابق للفيديو تماماً ===== */
*{box-sizing:border-box;margin:0;padding:0}
body{background:#0b0f19;color:#fff;font-family:Arial,sans-serif;min-height:100vh;display:flex;flex-direction:column;overflow-x:hidden}

/* شريط جيمايلوت العلوي الملون - مثل الفيديو */
.header{height:56px;background:linear-gradient(90deg,#facc15 0%,#f97316 25%,#22c55e 50%,#3b82f6 100%);display:flex;align-items:center;justify-content:space-between;padding:0 12px;position:fixed;top:0;left:0;width:100%;z-index:1000;box-shadow:0 2px 15px rgba(0,0,0,.3)}
.header-title{font-size:19px;font-weight:900;color:#fff;text-shadow:0 1px 3px rgba(0,0,0,.3);letter-spacing:.5px}
.header-actions{display:flex;gap:6px;align-items:center}
.menu-btn{width:38px;height:38px;background:rgba(0,0,0,.28);border:none;border-radius:10px;cursor:pointer;display:flex;flex-direction:column;justify-content:center;align-items:center;gap:4px;color:#fff;font-size:16px;transition:.2s}
.menu-btn:hover{background:rgba(0,0,0,.4);transform:scale(1.05)}
.menu-btn span{display:block;width:18px;height:2.5px;background:#fff;border-radius:2px}
.counter-badge{background:rgba(0,0,0,.45);padding:5px 10px;border-radius:12px;font-size:11px;color:#facc15;font-weight:bold;border:1px solid rgba(250,204,21,.4);box-shadow:0 1px 5px rgba(0,0,0,.2)}

/* شريط النماذج - من اليمين لليسار بداية من النورمال */
.models-bar{position:fixed;top:56px;left:0;width:100%;display:flex;gap:8px;padding:10px 12px;overflow-x:auto;background:#0b0f19;border-bottom:1px solid #1f2937;z-index:950;scrollbar-width:none;direction:rtl}
.models-bar::-webkit-scrollbar{display:none}
.model-btn{flex-shrink:0;padding:9px 15px;border-radius:20px;border:1.5px solid rgba(255,255,255,.15);background:transparent;color:#e5e7eb;font-size:11px;font-weight:700;cursor:pointer;white-space:nowrap;transition:.25s}
.model-btn:hover{border-color:rgba(250,204,21,.4);background:rgba(255,255,255,.06);transform:translateY(-1px)}
.model-btn.active{background:linear-gradient(90deg,#facc15,#f97316,#22c55e,#3b82f6);color:#111;border-color:transparent;box-shadow:0 3px 15px rgba(250,204,21,.4);font-weight:900}
.model-btn.locked{opacity:.85}

/* القائمة الجانبية */
.sidebar{position:fixed;top:56px;left:0;width:285px;height:calc(100vh - 56px);background:#111827;border-right:1px solid #1f2937;z-index:900;transform:translateX(-100%);transition:transform .32s cubic-bezier(0.4,0,0.2,1);overflow-y:auto;padding:14px;box-shadow:4px 0 20px rgba(0,0,0,.3)}
.sidebar.open{transform:translateX(0)}
.sidebar h3{font-size:14px;margin-bottom:10px;color:#facc15}
.chat-item{display:flex;align-items:center;justify-content:space-between;padding:11px;border-radius:11px;background:#1f2937;margin-bottom:6px;font-size:13px;transition:.2s}
.chat-item:hover{background:#262f3f}
.chat-item .chat-title{flex:1;cursor:pointer;overflow:hidden;text-overflow:ellipsis;white-space:nowrap}
.delete-btn{background:none;border:none;color:#ef4444;font-size:15px;cursor:pointer;padding:2px 6px}
.new-chat-btn{width:100%;padding:11px;border:none;border-radius:11px;background:linear-gradient(90deg,#facc15,#f97316);color:#111;font-weight:900;cursor:pointer;margin-bottom:14px;box-shadow:0 2px 10px rgba(250,204,21,.25);transition:.2s}
.new-chat-btn:hover{transform:translateY(-1px);box-shadow:0 4px 15px rgba(250,204,21,.35)}

/* المحادثة */
.chat-container{flex:1;width:100%;max-width:800px;margin:70px auto 180px;padding:16px;display:flex;flex-direction:column}
.welcome-box{text-align:center;margin:auto;padding:50px 0;animation:fadeIn .6s ease}
@keyframes fadeIn{from{opacity:0;transform:translateY(10px)}to{opacity:1;transform:translateY(0)}}
.welcome-box h1{font-size:32px;margin-bottom:8px;font-weight:900;letter-spacing:.5px}
.welcome-box .line{width:50px;height:3px;background:linear-gradient(90deg,#facc15,#f97316,#22c55e,#3b82f6);margin:0 auto 12px;border-radius:2px}
.welcome-box p{color:#9ca3af;font-size:14px}
.limit-info{margin-top:18px;padding:12px 14px;background:linear-gradient(135deg,#111827,#1f2937);border-radius:12px;border:1px solid #1f2937;font-size:11px;line-height:1.6;color:#9ca3af;box-shadow:0 2px 10px rgba(0,0,0,.2)}

.message{max-width:86%;padding:13px 15px;margin:9px 0;border-radius:18px;font-size:14px;line-height:1.7;word-wrap:break-word;white-space:pre-wrap;animation:msgIn .35s ease}
@keyframes msgIn{from{opacity:0;transform:translateY(8px) scale(.98)}to{opacity:1;transform:translateY(0) scale(1)}}
.message.user{background:linear-gradient(90deg,#facc15,#f97316,#22c55e,#3b82f6);color:#fff;font-weight:700;margin-right:auto;border-bottom-left-radius:5px;box-shadow:0 3px 15px rgba(250,204,21,.25)}
.message.ai{background:#fff;color:#111;margin-left:auto;border-bottom-right-radius:5px;box-shadow:0 3px 12px rgba(0,0,0,.15)}
.message.ai img,.message.ai video{max-width:100%;border-radius:12px;margin-top:8px;display:block}
.message.blocked{background:#7f1d1d;color:#fff;margin:0 auto;max-width:95%;text-align:center;border-radius:12px}
.source-tag{font-size:9px;color:#6b7280;margin-top:7px;display:block;border-top:1px solid #e5e7eb;padding-top:5px}

/* اقتراحات */
.suggestions{position:fixed;bottom:88px;left:0;width:100%;display:flex;gap:8px;padding:8px 12px;overflow-x:auto;background:transparent;z-index:999;scrollbar-width:none;direction:rtl}
.suggestions::-webkit-scrollbar{display:none}
.sug-btn{flex-shrink:0;padding:9px 15px;border-radius:20px;border:1px solid rgba(255,255,255,.15);background:rgba(255,255,255,.09);color:#e5e7eb;font-size:11px;font-weight:600;cursor:pointer;backdrop-filter:blur(8px);transition:.25s}
.sug-btn:hover{background:rgba(250,204,21,.18);border-color:#facc15;transform:translateY(-2px);box-shadow:0 3px 10px rgba(250,204,21,.2)}

/* شريط الكتابة - تأثيرات الألوان */
.input-area{position:fixed;bottom:0;left:0;width:100%;padding:10px 12px 14px;background:linear-gradient(to top,#0b0f19 85%,transparent);z-index:1000}
.gemini-bar{max-width:740px;margin:0 auto;display:flex;align-items:center;background:#ffffff;border-radius:34px;padding:6px 8px;gap:6px;box-shadow:0 8px 30px rgba(0,0,0,.35),0 0 0 1px rgba(0,0,0,.05);transition:all .32s cubic-bezier(0.4,0,0.2,1)}
.gemini-bar:focus-within{box-shadow:0 12px 40px rgba(0,0,0,.45),0 0 0 2.5px rgba(250,204,21,.55),0 0 20px rgba(250,204,21,.2);transform:translateY(-1px)}
.plus-btn{width:42px;height:42px;border-radius:50%;border:none;background:#fff;color:#111;font-size:26px;cursor:pointer;display:flex;align-items:center;justify-content:center;transition:.2s}
.plus-btn:hover{background:#f3f4f6;transform:rotate(90deg)}
.input-wrapper{flex:1;display:flex;align-items:center;min-width:0}
.input-wrapper textarea{flex:1;border:none;outline:none;resize:none;font-size:15px;color:#111;background:transparent;max-height:90px;padding:11px 2px;line-height:1.45}
.input-wrapper textarea::placeholder{color:#6b7280}
.mic-btn{width:42px;height:42px;border-radius:50%;border:none;background:transparent;color:#111;font-size:20px;cursor:pointer;display:flex;align-items:center;justify-content:center;transition:.2s}
.mic-btn:hover{background:#f3f4f6}
.send-btn{width:46px;height:46px;border-radius:50%;border:none;cursor:pointer;display:flex;align-items:center;justify-content:center;transition:.28s}
.send-btn.wave{background:#e0f2fe;color:#111;font-size:16px}
.send-btn.active{background:linear-gradient(135deg,#facc15,#f97316,#22c55e,#3b82f6);color:#111;font-size:18px;box-shadow:0 3px 15px rgba(250,204,21,.4);transform:scale(1.05)}
.send-btn.active:hover{transform:scale(1.1)}

/* النوافذ */
.tools-popup{position:fixed;bottom:92px;left:50%;transform:translateX(-50%);max-width:360px;width:92%;background:#111827;border:1px solid #1f2937;border-radius:18px;padding:10px;display:none;z-index:1001;box-shadow:0 20px 50px rgba(0,0,0,.6);animation:popIn .28s ease}
@keyframes popIn{from{opacity:0;transform:translateX(-50%) translateY(10px) scale(.96)}to{opacity:1;transform:translateX(-50%) translateY(0) scale(1)}}
.tools-popup.open{display:block}
.tools-popup button{width:100%;text-align:right;padding:11px 12px;border-radius:11px;border:none;background:transparent;color:#e5e7eb;font-size:12px;cursor:pointer;display:flex;align-items:center;gap:9px;transition:.2s}
.tools-popup button:hover{background:#1f2937;transform:translateX(-2px)}

/* تأثيرات الألوان الكاملة */
.full-glow{position:fixed;inset:0;pointer-events:none;z-index:9999;opacity:0;background:linear-gradient(120deg,#facc15 0%,#f97316 25%,#22c55e 50%,#3b82f6 75%,#facc15 100%);background-size:400% 400%;mix-blend-mode:screen}
.full-glow.active{animation:fullGlowFlow 1.8s ease-in-out forwards}
@keyframes fullGlowFlow{0%{background-position:0% 50%;opacity:.7}50%{background-position:80% 50%;opacity:.9}100%{background-position:0% 50%;opacity:0}}

.edit-modal{position:fixed;inset:0;background:rgba(0,0,0,.88);z-index:2000;display:none;align-items:center;justify-content:center;padding:12px;backdrop-filter:blur(4px)}
.edit-box{background:#111827;border-radius:18px;padding:18px;width:100%;max-width:520px;max-height:90vh;overflow:auto;border:1px solid #1f2937;box-shadow:0 20px 60px rgba(0,0,0,.6)}
.edit-box canvas{width:100%;border-radius:12px;background:#000;margin:12px 0}
.edit-tools{display:flex;flex-wrap:wrap;gap:7px}
.edit-tools button{padding:7px 11px;border-radius:9px;border:1px solid #374151;background:#1f2937;color:#fff;font-size:11px;cursor:pointer;transition:.2s}
.edit-tools button:hover{background:#374151;transform:translateY(-1px)}
</style>
</head>
<body>
<!-- شاشة ترحيبية بستايلك الأصلي -->
<div class="ai-splash-screen" id="aiSplash">
  <div class="ai-splash-glow"></div>
  <div class="ai-icon-container">
    <img src="icon.png" class="ai-splash-icon" alt="Alislamiah Icon">
  </div>
  <div class="ai-splash-text">كيف يمكنني مساعدتك</div>
</div>
<script>
(function(){
  var s=document.getElementById('aiSplash');
  if(!s) return;
  function hide(){s.classList.add('hidden');setTimeout(function(){s.style.display='none';try{s.remove()}catch(e){}},850);}
  setTimeout(hide,5000);
  s.addEventListener('click',hide);
})();
</script>

<!-- شريط Gemalot العلوي الملون - بدون ساعة -->
<div class="header">
  <div style="display:flex;gap:6px;align-items:center">
    <button class="menu-btn" onclick="toggleSidebar()" title="قائمة المحادثات"><span></span><span></span><span></span></button>
    <button id="settingsBtn" class="menu-btn" onclick="location.href='settings.html'" style="background:rgba(255,255,255,.18)" title="الإعدادات">⚙️</button>
    <div id="msgCounter" class="counter-badge">200/200</div>
  </div>
  <div style="display:flex;align-items:center;gap:6px">
    <div class="header-title" style="font-size:14px">Gemalot</div>
    <button class="model-pill" onclick="toggleModelDropdown()" id="currentModelBtn" style="display:flex;align-items:center;gap:6px;background:rgba(0,0,0,.32);border:none;border-radius:22px;padding:8px 12px;color:#fff;font-size:12px;font-weight:700;cursor:pointer">▼ Gemalot Normal</button>
  </div>
</div>

<!-- Dropdown مطابق للصورة -->
<div class="model-dropdown" id="modelDropdown" style="position:fixed;top:62px;right:8px;left:8px;max-width:420px;margin:0 auto;background:#fff;border-radius:22px;box-shadow:0 20px 50px rgba(0,0,0,.45);padding:8px;display:none;z-index:5000;color:#111;max-height:82vh;overflow-y:auto">
  <div class="model-opt active" data-model="normal" onclick="selectModelFromDropdown('normal', event)" style="display:flex;align-items:center;justify-content:space-between;padding:16px 14px;border-radius:16px;background:#fef9c3;cursor:pointer">
    <div style="flex:1;text-align:right"><div style="font-weight:800;font-size:16px;color:#111">Gemalot Normal</div><div style="font-size:12px;color:#6b7280;margin-top:4px">200 رسالة - بدون Gemini - الوضع العادي</div></div>
    <div class="model-check" style="margin-left:12px;color:#22c55e;font-size:20px;font-weight:bold">✓</div>
  </div>
  <div class="model-opt" data-model="pro" onclick="selectModelFromDropdown('pro', event)" style="display:flex;align-items:center;justify-content:space-between;padding:16px 14px;border-radius:16px;cursor:pointer">
    <div style="flex:1;text-align:right"><div style="font-weight:700;font-size:15px;color:#111">Gemalot Pro</div><div style="font-size:12px;color:#6b7280;margin-top:4px">15 رسالة - الوضع العادي - Gemini</div></div>
  </div>
  <div style="height:1px;background:#e5e7eb;margin:12px 8px"></div>
  <div class="model-opt" onclick="location.href='subscription.html'" style="padding:16px 14px;border-radius:16px;cursor:pointer">
    <div style="flex:1;text-align:right"><div style="font-weight:700;font-size:15px;color:#111">Gemalot Pro Plus 🔒</div><div style="font-size:12px;color:#6b7280;margin-top:4px">25 رسالة + مصادر أكثر + Gemini - اشتراك</div></div>
  </div>
  <div class="model-opt" onclick="location.href='subscription.html'" style="padding:16px 14px;border-radius:16px;cursor:pointer">
    <div style="flex:1;text-align:right"><div style="font-weight:700;font-size:15px;color:#111">Gemalot Pro Gold 🔒</div><div style="font-size:12px;color:#6b7280;margin-top:4px">35 رسالة + فاكثر + Gemini - اشتراك</div></div>
  </div>
  <div class="model-opt" onclick="location.href='subscription.html'" style="padding:16px 14px;border-radius:16px;cursor:pointer">
    <div style="flex:1;text-align:right"><div style="font-weight:700;font-size:15px;color:#111">Gemalot Pro Extra 🔒</div><div style="font-size:12px;color:#6b7280;margin-top:4px">50 رسالة + حزمة كاملة + Gemini - اشتراك</div></div>
  </div>
</div>

<div style='height:56px'></div>

<div id="sidebar" class="sidebar">
  <button class="new-chat-btn" onclick="newChat()">+ محادثة جديدة</button>
  <h3>المحادثات السابقة</h3>
  <div id="chatList"></div>
  <div style="margin-top:20px;padding:12px;background:linear-gradient(135deg,#0b0f19,#111827);border-radius:11px;border:1px solid #1f2937;font-size:11px;color:#9ca3af">
    <div id="subStatus">الوضع: عادي (200 رسالة/يوم)</div>
    <div id="subExpiry" style="margin-top:4px"></div>
  </div>
</div>

<div id="chatContainer" class="chat-container">
  <div id="welcomeBox" class="welcome-box">
    <h1>Gemalot</h1>
    <div class="line"></div>
    <p id="welcomeText">كيف يمكنني مساعدتك؟</p>
    <div class="limit-info" id="limitInfo">متبقي: 200/200 اليوم</div>
  </div>
</div>

<div id="suggestionsBar" class="suggestions"></div>

<div class="tools-popup" id="toolsPopup">
  <button onclick="triggerUpload('image');closeTools()"><span>🖼️</span> رفع صورة وتعديلها</button>
  <button onclick="triggerUpload('video');closeTools()"><span>🎥</span> رفع فيديو</button>
  <button onclick="askImageGen();closeTools()"><span>🎨</span> إنشاء صورة 4K</button>
  <button onclick="askVideoGen();closeTools()"><span>🎬</span> إنشاء فيديو</button>
  <button onclick="insertPrompt('حل المعادلة: ');closeTools()"><span>🧮</span> حل معادلة</button>
  <button onclick="askTranslate();closeTools()"><span>🌐</span> Eisen.translastor - ترجمة من الموقع</button>
</div>

<div class="input-area">
  <div class="gemini-bar" id="geminiBar">
    <button class="plus-btn" id="plusBtn" onclick="toggleTools()">+</button>
    <div id="inputWrapper" class="input-wrapper"><textarea id="userInput" placeholder="اسأل Gemalot" rows="1"></textarea></div>
    <button class="mic-btn" id="micBtn" onclick="toggleMic()">🎙️</button>
    <button id="sendBtn" class="send-btn wave"><span id="sendIcon">◍</span></button>
  </div>
</div>

<input type="file" id="fileInput" hidden accept="image/*,video/*">
<div id="fullGlow" class="full-glow"></div>
<div id="editModal" class="edit-modal"><div class="edit-box"><h3 style="color:#facc15;font-size:14px">محرر احترافي</h3><canvas id="editCanvas"></canvas><div class="edit-tools"><button onclick="applyFilter('grayscale')">أبيض وأسود</button><button onclick="applyFilter('sepia')">Sepia</button><button onclick="applyFilter('invert')">عكسي</button><button onclick="applyFilter('bright')">تفتيح</button><button onclick="rotateCanvas(90)">دوران</button><button onclick="addTextOverlay()">نص</button><button onclick="downloadCanvas()">تحميل</button><button onclick="closeEditor()">إغلاق</button></div></div></div>

<script>
const planLimits={normal:200, pro:15, pro_plus:25, pro_gold:35, pro_extra:50};
const translations={
  ar:{dir:"rtl",welcome:"كيف يمكنني مساعدتك؟",placeholder:"اسأل Gemalot",newChat:"+ محادثة جديدة",prevChats:"المحادثات السابقة",suggestions:["أنشئ صورة: مسجد مستقبلي","أنشئ فيديو: محيط","حل المعادلة: 2x+3=11","نواقض الوضوء","أحكام الصيام"],noAnswer:"عذراً، لم أجد إجابة دقيقة.",searching:"جاري البحث...",blockedTitle:"انتهت رسائلك",remaining:"متبقي: {n}/{t} اليوم"},
  fr:{dir:"ltr",welcome:"Comment puis-je vous aider ?",placeholder:"Ask Gemalot",newChat:"+ Nouveau chat",prevChats:"Conversations",suggestions:["générer image: mosquée","résoudre: 2x+3=11"],noAnswer:"Désolé, pas de réponse exacte.",searching:"Recherche...",blockedTitle:"Limite atteinte",remaining:"Restant: {n}/{t}"},
  en:{dir:"ltr",welcome:"How can I help you?",placeholder:"Ask Gemalot",newChat:"+ New Chat",prevChats:"Previous",suggestions:["generate image: futuristic mosque","solve: 2x+3=11"],noAnswer:"No exact answer.",searching:"Searching...",blockedTitle:"Limit reached",remaining:"Remaining: {n}/{t}"}
};
let currentLang=localStorage.getItem("gemalot_language")||"ar";
let t=translations[currentLang]||translations.ar;
function detectLang(text){
  if(/[\u0600-\u06FF]/.test(text)) return 'ar';
  if(/[àâéèêëîïôùûüÿç]/i.test(text)) return 'fr';
  return 'en';
}
function parseCurrentUser(){const raw=localStorage.getItem("alislamiah_current_user")||"";if(!raw)return {raw:"",name:"",email:"",id:""};try{const j=JSON.parse(raw);if(j&&typeof j==="object")return {raw,name:j.name||j.username||"",email:j.email||"",id:String(j.id||"")};}catch{}return {raw,name:raw,email:"",id:""};}
function getSubscription(){const all=JSON.parse(localStorage.getItem("gemalot_all_subscriptions")||"{}");const cu=parseCurrentUser();if(all[cu.raw])return all[cu.raw];if(cu.name&&all[cu.name])return all[cu.name];if(cu.email&&all[cu.email])return all[cu.email];return null;}
function getDailyLimit(){
  const sub=getSubscription();
  if(sub&&sub.dailyLimit)return parseInt(sub.dailyLimit);
  if(sub&&sub.plan&&planLimits[sub.plan])return planLimits[sub.plan];
  // Use currently selected model
  const currentModel = localStorage.getItem('gemalot_current_model') || 'normal';
  if(planLimits[currentModel]) return planLimits[currentModel];
  return planLimits.normal;
}
function isSubscribed(){const sub=getSubscription();if(sub&&sub.expiry){if(Date.now()<parseInt(sub.expiry))return true;}return localStorage.getItem("gemalot_subscription_active")==="true";}
function getTodayStr(){return new Date().toISOString().split('T')[0];}
function checkAndResetDaily(){const d=localStorage.getItem("gemalot_daily_date");const today=getTodayStr();if(d!==today){localStorage.setItem("gemalot_daily_date",today);localStorage.setItem("gemalot_daily_count","0");}}
function getBlockUntil(){return parseInt(localStorage.getItem("gemalot_block_until")||"0");}
function canSendMessage(){checkAndResetDaily();if(isSubscribed())return {allowed:true};const b=getBlockUntil();if(b&&Date.now()<b)return {allowed:false};const c=parseInt(localStorage.getItem("gemalot_daily_count")||"0");if(c>=getDailyLimit())return {allowed:false};return {allowed:true};}
function incrementCount(){if(isSubscribed())return;let c=parseInt(localStorage.getItem("gemalot_daily_count")||"0");c++;localStorage.setItem("gemalot_daily_count",c.toString());}
function applyLanguage(lang){currentLang=lang;t=translations[lang]||translations.ar;localStorage.setItem("gemalot_language",lang);document.documentElement.lang=lang;document.documentElement.dir=t.dir;const wt=document.getElementById("welcomeText");if(wt)wt.textContent=t.welcome;updatePlaceholder();updateLimitUI();renderSuggestions();}
function renderSuggestions(){const bar=document.getElementById("suggestionsBar");if(!bar)return;bar.innerHTML="";(t.suggestions||[]).forEach(s=>{const b=document.createElement("button");b.className="sug-btn";b.textContent=s;b.onclick=()=>sendSuggestion(s);bar.appendChild(b);});}
function triggerFullGlow(){const g=document.getElementById("fullGlow");if(!g)return;g.classList.remove("active");void g.offsetWidth;g.classList.add("active");}
function getChats(){return JSON.parse(localStorage.getItem("gemalot_chats")||"[]");}
function saveChats(c){localStorage.setItem("gemalot_chats",JSON.stringify(c));}
function renderChatList(){const cl=document.getElementById("chatList");if(!cl)return;cl.innerHTML="";getChats().forEach(c=>{const div=document.createElement("div");div.className="chat-item";div.innerHTML=`<span class="chat-title">${c.title||"محادثة"}</span><button class="delete-btn">🗑️</button>`;div.querySelector('.chat-title').onclick=()=>loadChat(c.id);div.querySelector('.delete-btn').onclick=e=>{e.stopPropagation();deleteChat(c.id);};cl.appendChild(div);});}
function deleteChat(id){saveChats(getChats().filter(c=>c.id!==id));if(currentChatId===id)newChat();renderChatList();}
function newChat(){currentChatId=Date.now().toString();messages=[];const cc=document.getElementById("chatContainer");if(cc)cc.innerHTML=`<div id="welcomeBox" class="welcome-box"><h1>Gemalot</h1><div class="line"></div><p id="welcomeText">${t.welcome}</p><div class="limit-info" id="limitInfo"></div></div>`;const sb=document.getElementById("sidebar");if(sb)sb.classList.remove("open");updateLimitUI();renderSuggestions();}
function loadChat(id){const chat=getChats().find(c=>c.id===id);if(!chat)return;currentChatId=id;messages=chat.messages||[];const cc=document.getElementById("chatContainer");if(!cc)return;cc.innerHTML="";messages.forEach(m=>appendMessage(m.text,m.sender,false,m.source,m.html));const sb=document.getElementById("sidebar");if(sb)sb.classList.remove("open");}
function saveCurrentChat(){if(!messages.length)return;const chats=getChats();const title=messages[0]?.text?.slice(0,30)||"محادثة";const data={id:currentChatId||Date.now().toString(),title,messages,updated:Date.now()};const i=chats.findIndex(c=>c.id===currentChatId);if(i>-1)chats[i]=data;else chats.unshift(data);saveChats(chats);renderChatList();}

function toggleModelDropdown(){
  var d=document.getElementById('modelDropdown');
  if(!d) return;
  d.style.display = d.style.display==='block'?'none':'block';
}
function selectModelFromDropdown(model, ev){
  if(ev){ ev.stopPropagation(); ev.preventDefault(); }
  if(model !== 'normal' && model !== 'pro') return;
  localStorage.setItem('gemalot_current_model', model);
  var dd = document.getElementById('modelDropdown');
  if(dd) dd.style.display = 'none';
  // Update button text
  var btn = document.getElementById('currentModelBtn');
  if(btn){
    btn.textContent = (model === 'normal') ? '▼ Gemalot Normal' : '▼ Gemalot Pro';
  }
  // Clear all options visual state
  document.querySelectorAll('.model-opt[data-model]').forEach(function(opt){
    opt.classList.remove('active');
    opt.style.background = '';
    // remove any existing check marks
    opt.querySelectorAll('.model-check').forEach(function(c){ c.remove(); });
  });
  // Activate selected
  var selected = document.querySelector('.model-opt[data-model="'+model+'"]');
  if(selected){
    selected.classList.add('active');
    selected.style.background = '#fef9c3';
    var checkDiv = document.createElement('div');
    checkDiv.className = 'model-check';
    checkDiv.style.cssText = 'margin-left:12px;color:#22c55e;font-size:20px;font-weight:bold';
    checkDiv.textContent = '✓';
    selected.appendChild(checkDiv);
  }
  updateLimitUI();
}
// Close dropdown when clicking outside
document.addEventListener('click', function(e){
  var dd = document.getElementById('modelDropdown');
  if(!dd) return;
  if(dd.style.display === 'none' || dd.style.display === '') return;
  if(e.target.closest('#modelDropdown') || e.target.closest('#currentModelBtn')) return;
  dd.style.display = 'none';
});

function toggleSidebar(){const sb=document.getElementById("sidebar");if(sb)sb.classList.toggle("open");}
function updateLimitUI(){checkAndResetDaily();const counter=document.getElementById("msgCounter");const subStatus=document.getElementById("subStatus");const limitInfo=document.getElementById("limitInfo");if(!counter)return;const lim=getDailyLimit();const count=parseInt(localStorage.getItem("gemalot_daily_count")||"0");const remaining=Math.max(0,lim-count);counter.textContent=`${remaining}/${lim}`;if(limitInfo)limitInfo.textContent=`متبقي: ${remaining}/${lim} اليوم - بدون سيرفر + Eisen.translastor`;if(subStatus)subStatus.textContent=`الوضع: عادي (${remaining}/${lim})`;}
function selectModel(btn){document.querySelectorAll('.model-btn').forEach(b=>b.classList.remove('active'));btn.classList.add('active');}
let messages=[];let currentChatId=null;
const chatContainer=document.getElementById('chatContainer');const userInput=document.getElementById('userInput');const sendBtn=document.getElementById('sendBtn');const fileInput=document.getElementById('fileInput');
function appendMessage(text,sender,save=true,source,htmlContent){
  const w=document.getElementById("welcomeBox");if(w)w.style.display="none";
  const div=document.createElement("div");div.className=`message ${sender}`;
  if(htmlContent){div.innerHTML=htmlContent;}
  else if(sender==="blocked"){div.innerHTML=text;}
  else{const span=document.createElement("span");span.textContent=text;div.appendChild(span);if(source){const tag=document.createElement("span");tag.className="source-tag";tag.textContent=`المصدر: ${source}`;div.appendChild(document.createElement("br"));div.appendChild(tag);}}
  chatContainer.appendChild(div);window.scrollTo({top:document.body.scrollHeight,behavior:"smooth"});
  if(save&&sender!=="blocked"){messages.push({text,sender,source,html:htmlContent||null});if(!currentChatId)currentChatId=Date.now().toString();saveCurrentChat();}
}
function triggerUpload(type){if(!fileInput)return;fileInput.setAttribute("data-type",type);fileInput.accept=type==="image"?"image/*":"video/*";fileInput.click();}
function fileToBase64(file){return new Promise((res,rej)=>{const r=new FileReader();r.onload=()=>res(r.result);r.onerror=rej;r.readAsDataURL(file);});}
if(fileInput)fileInput.addEventListener("change", async (e)=>{
  const file=e.target.files[0];if(!file)return;
  if(file.size>8*1024*1024){alert('الملف كبير');e.target.value='';return;}
  try{
    const base64=await fileToBase64(file);
    if(file.type.startsWith("image/")){
      const html=`<div>🖼️ ${file.name}</div><img src="${base64}" style="max-width:100%;border-radius:12px;margin-top:8px" onclick="openEditor(this.src)">`;
      appendMessage(file.name,"user",true,null,html);
      appendMessage("✅ تم رفع الصورة!","ai",true,"محلي");
    }else{
      const html=`<div>🎥 ${file.name}</div><video src="${base64}" controls style="max-width:100%;border-radius:12px"></video>`;
      appendMessage(file.name,"user",true,null,html);
      appendMessage("✅ تم رفع الفيديو!","ai",true,"محلي");
    }
  }catch{alert('خطأ في الرفع');}
  e.target.value='';
});
async function askTranslate(){const p=prompt("النص للترجمة (Eisen.translastor):");if(p){const to=prompt("إلى أي لغة؟ ar/en/fr/es (افتراضي: en)","en")||"en";const res=await fetchFromEisen(p,to);if(res){appendMessage(p,"user");appendMessage(res.text,"ai",true,res.source);}else{alert("تعذر جلب الترجمة من Eisen");}}}
function askImageGen(){const p=prompt("وصف الصورة:");if(p)processMessage("صورة: "+p);}
function askVideoGen(){const p=prompt("وصف الفيديو:");if(p)processMessage("فيديو: "+p);}
function insertPrompt(txt){if(!userInput)return;userInput.value=txt;userInput.focus();updateSendIcon();}
function generateImagePollinations(prompt){const seed=Math.floor(Math.random()*100000);const enc=encodeURIComponent(prompt+", ultra detailed, 4k");return `https://image.pollinations.ai/prompt/${enc}?width=1024&height=1024&seed=${seed}&nologo=true`;}
function generateVideoCanvas(prompt){const canvas=document.createElement("canvas");canvas.width=640;canvas.height=360;const ctx=canvas.getContext("2d");let frame=0;return new Promise(resolve=>{const stream=canvas.captureStream(30);const recorder=new MediaRecorder(stream,{mimeType:"video/webm"});const chunks=[];recorder.ondataavailable=e=>chunks.push(e.data);recorder.onstop=()=>{const blob=new Blob(chunks,{type:"video/webm"});resolve(URL.createObjectURL(blob));};recorder.start();function draw(){ctx.fillStyle=`hsl(${(frame*2)%360},70%,30%)`;ctx.fillRect(0,0,640,360);ctx.fillStyle="#fff";ctx.font="bold 24px Arial";ctx.textAlign="center";ctx.fillText(prompt.slice(0,40),320,180);frame++;if(frame<60){requestAnimationFrame(draw);}else{setTimeout(()=>recorder.stop(),200);}}draw();});}
function solveMathPro(text){
  try{
    let expr=text.replace("حل المعادلة:","").replace("حل:","").replace("solve:","").trim();
    if(!expr) return null;
    if(expr.includes('=') && expr.toLowerCase().includes('x')){
      let parts=expr.split('=');
      if(parts.length==2){
        try{
          let b = math.evaluate(parts[0].replace(/x/g,'(0)'));
          let a_plus_b = math.evaluate(parts[0].replace(/x/g,'(1)'));
          let a = a_plus_b - b;
          let c = math.evaluate(parts[1]);
          let x = (c - b)/a;
          return `🧮 ${expr} => x = ${x}`;
        }catch{}
      }
    }
    if(/^[0-9x+\-*/^().=\s]+$/.test(expr)){
      const res=math.evaluate(expr.replace(/=/g,''));
      return `🧮 ${expr} = ${res}`;
    }
    return null;
  }catch{return null;}
}
async function safeFetch(url){try{const r=await fetch(url);if(!r.ok)return null;return r;}catch{return null;}}
// Eisen.translastor - يجيب الترجمة من الموقع مباشرة (فقط عند اختياره من قائمة +)
async function fetchFromEisen(text, targetLang){
  try{
    let src = detectLang(text);
    let to = (targetLang||'').toLowerCase() || (src==='ar' ? 'en' : 'ar');
    if(src===to){
      if(to==='ar') to='en';
      else if(to==='en') to='ar';
      else to='en';
    }
    const url = `https://api.mymemory.translated.net/get?q=${encodeURIComponent(text)}&langpair=${src}|${to}`;
    const res = await safeFetch(url);
    if(res){
      const data = await res.json();
      if(data.responseData && data.responseData.translatedText){
        let t = data.responseData.translatedText;
        if(t.includes("PLEASE SELECT") || t.includes("TWO DISTINCT")) return null;
        if(t.toLowerCase() !== text.toLowerCase() && t.length>0 && t.length<1000){
          return {text: t, source: `Eisen.translastor (${src}→${to}) - https://mohamedmadoui5195.github.io/Eisen.translastor/`};
        }
      }
    }
  }catch{}
  return null;
}

// ===== دوال البرمجة اللي أضفتها - MDN + DuckDuckGo =====
async function fetchGemalotMultiSource(userQuery){
  const proxyUrl = "https://api.allorigins.win/get?url=";
  const sources = [
    {type:"mdn", url: `https://developer.mozilla.org/api/v1/search?q=${encodeURIComponent(userQuery)}`},
    {type:"duckduckgo", url: `https://api.duckduckgo.com/?q=${encodeURIComponent(userQuery + " programming code")}&format=json`}
  ];
  try{
    for(let source of sources){
      try{
        let response = await fetch(proxyUrl + encodeURIComponent(source.url));
        let wrapper = await response.json();
        if(wrapper.contents){
          let data = JSON.parse(wrapper.contents);
          if(source.type==="mdn" && data.documents && data.documents.length>0){
            let doc = data.documents[0];
            return `📚 **من التوثيق التقني (MDN):**\n\n**${doc.title}**\n${doc.summary}\n\n🔗 [رابط التوثيق](https://developer.mozilla.org${doc.mdn_url})`;
          }
          if(source.type==="duckduckgo"){
            if(data.RelatedTopics && data.RelatedTopics.length>0){
              return `💡 **من المصادر البرمجية المفتوحة:**\n\n${data.RelatedTopics[0].Text}`;
            } else if(data.AbstractText){
              return `📖 **من المعرفة العامة والتقنية:**\n\n${data.AbstractText}`;
            }
          }
        }
      }catch(e){ continue; }
    }
    return null;
  }catch(error){
    return null;
  }
}

async function searchWikipediaLang(q,lang){try{let res=await safeFetch(`https://${lang}.wikipedia.org/api/rest_v1/page/summary/${encodeURIComponent(q)}`);if(res){const d=await res.json();if(d.extract&&d.extract.length>40)return {text:d.extract,source:`Wikipedia ${lang}`};}res=await safeFetch(`https://${lang}.wikipedia.org/w/api.php?action=query&list=search&srsearch=${encodeURIComponent(q)}&format=json&origin=*`);if(res){const d=await res.json();const first=d.query?.search?.[0]?.title;if(first){res=await safeFetch(`https://${lang}.wikipedia.org/api/rest_v1/page/summary/${encodeURIComponent(first)}`);if(res){const dd=await res.json();if(dd.extract)return {text:dd.extract,source:`Wikipedia ${lang}`};}}}}catch{}return null;}

async function searchAllSources(q,preferredLang){
  // مصادر عادية فقط - الترجمة لا تعمل تلقائيا
  // الترجمة تعمل فقط عند اختيارها من قائمة + (askTranslate)
  let langs=["ar","en","fr","es","tr","de","id","ur","fa"];
  if(preferredLang) langs=[preferredLang,...langs.filter(l=>l!==preferredLang)];
  for(let l of langs){
    const r=await searchWikipediaLang(q,l);
    if(r) return r;
  }
  return null;
}
const chatKnowledge = [
  {roots: ["كيف يمكنك مساعدتك", "مرحبا", "السلام عليكم", "أهلا"], reply: "وعليكم السلام! أنا Gemalot مساعدك الذكي Alislamiah-AI، جاهز لإجابتك."},
  {roots: ["نواقض الوضوء"], reply: "نواقض الوضوء: الخارج من السبيلين، زوال العقل، مس الفرج بشهوة، أكل لحم الإبل، وغيرها."},
  {roots: ["أحكام الصيام", "الصيام"], reply: "الصيام ركن من أركان الإسلام، شروطه الإسلام والبلوغ والعقل والإقامة والصحة."},
  {roots: ["أحكام سجود السهو"], reply: "سجود السهو مشروع لجبر ما حصل في الصلاة من زيادة أو نقص."},
  {roots: ["Gemalot", "من أنت"], reply: "Gemalot هو مساعدك الذكي، تم تطويري بواسطة شبكة Alislamiah."},
  {roots: ["كيف أرسل رسالة في بريدكم", "كيف أرسل رسائل في بريد أيزن", "طريقة إرسال رسالة في بريد ايزن", "كيفية ارسال رسالة Eisen mail"], reply: "لإرسال رسالة عبر البريد:\n1. اضغط على زر (جديد) الموجود في القائمة السفلية.\n2. اكتب بريد الشخص الذي تود الإرسال إليه.\n3. أضف موضوع الرسالة (العنوان أو الفكرة العامة).\n4. اكتب نص الرسالة (المحتوى الذي تريد إرساله)."},
  {roots: ["الوقت", "أهمية الوقت", "تعبير عن الوقت", "الوقت وأهميته", "استثمار الوقت", "استغلال الوقت"], reply: "يعد الوقت من أثمن النعم التي يمتلكها الإنسان، فهو أساس النجاح والتقدم في الحياة.\n\nتكمن أهمية الوقت في أنه يساعد على تحقيق الأهداف وإنجاز الأعمال وتنظيم شؤون الحياة. ويمكن استغلاله من خلال وضع خطة يومية، وتحديد الأولويات، والابتعاد عن الملهيات، والالتزام بالمواعيد. كما أن استثمار الوقت في التعلم والعمل النافع يعود بالفائدة على الفرد والمجتمع."},
  {roots: ["الوطن", "حب الوطن", "تعبير عن حب الوطن", "تعبير عن الوطن", "أهمية الوطن", "واجبنا نحو الوطن"], reply: "الوطن ليس مجرد أرض نعيش فوقها، بل هو تاريخنا وذكرياتنا وهويتنا التي نفخر بها بين الأمم."},
  {roots: ["تزخر ولاية الجزائر العاصمة", "تراث الجزائري", "معالم الجزائر العاصمة", "قصبة الجزائر", "تراث العاصمة الجزائرية"], reply: "تزخر ولاية الجزائر بتراثٍ تاريخي وثقافي عريق يعكس تعاقب الحضارات التي مرت بها عبر القرون."},
  {roots: ["Alislamiah AI", "ماهي Alislamiah AI", "Alislamiah bing", "الإسلامية أي أي", "شبكة الإسلامية"], reply: "Alislamiah AI هو محرك البحث الذكي المطور بواسطة شبكة Alislamiah."},
  {roots: ["Gemalot", "Gemalot AI", "جيمايلوت", "من أنت", "من انت"], reply: "Gemalot هو مساعدك الذكي الذاتي، تم تطويري بواسطة شبكة Alislamiah لخدمتك وإجابة استفساراتك."},
  {roots: ["من الذي صنعك", "من هو مطورك", "المدير العام لشبكة الإسلامية", "من طورك", "من صنعك"], reply: "أنا نموذج ذكاء اصطناعي نصي تم تطويري بواسطة شبكة Alislamiah.\nمطوري هو المدير العام لشبكة الإسلامية الرقمية (The General Director of Alislamiah Digital Network)."},
  {roots: ["Eisen mail", "ماهو Eisen mail", "Alislamiah mail", "بريد ايسن", "بريد الإسلامية"], reply: "Eisen Mail هو تطبيق بريد إلكتروني جديد تم تطويره بواسطة Eisen التابعة لشبكة Alislamiah."},
  {roots: ["التدخين", "تعبير عن التدخين", "أضرار التدخين", "مخاطر التدخين", "الإقلاع عن التدخين", "التدخين السلبي"], reply: "يُعدّ التدخين من أكثر الظواهر انتشارًا في المجتمعات المعاصرة، وهو عبارة عن استنشاق الدخان الناتج عن احتراق التبغ."},
  {roots: ["المخدرات", "آفة المخدرات", "خطر المخدرات", "أضرار المخدرات", "الإدمان", "مكافحة المخدرات"], reply: "تُعتبر المخدرات من أخطر الآفات الاجتماعية التي تهدد الفرد والمجتمع."},
  {roots: ["الخمر", "أضرار الخمر", "مخاطر الخمر", "شرب الكحول", "تعبير عن الخمر", "أضرار الكحول"], reply: "الخمر مشروب يحتوي على نسبة من الكحول تؤثر في العقل والسلوك عند تناوله."},
  {roots: ["الغش", "أضرار الغش", "مخاطر الغش", "الغش في الدراسة", "الغش التجاري", "تعبير عن الغش", "الامانة والنزاهة"], reply: "الغش سلوك غير أخلاقي يقوم على الخداع والتلاعب بهدف الحصول على منفعة."},
  {roots: ["الرشوة", "خطر الرشوة", "مظاهر الفساد", "أضرار الرشوة", "الشفافية والنزاهة", "تعبير عن الرشوة"], reply: "تُعرّف الرشوة بأنها تقديم مال أو منفعة أو خدمة لشخص ما بطريقة غير قانونية."},
  {roots: ["أطفال الشوارع", "ظاهرة أطفال الشوارع", "أسباب أطفال الشوارع", "مخاطر أطفال الشوارع", "تعبير عن أطفال الشوارع", "رعاية أطفال الشوارع"], reply: "يُطلق مصطلح أطفال الشوارع على الأطفال الذين يقضون معظم أوقاتهم في الشوارع."},
  {roots: ["كيف أستخدم بريد إيزن", "كيف أستخدم أيزن مايل", "كيف أستخدم بريد الإسلامية", "كيف استخدم Eisen mail", "طريقة استخدام بريد ايزن", "ماذا أفعل لأستخدم Eisen mail"], reply: "استخدام Eisen Mail كالتالي: سجل الدخول إلى حسابك أو أنشئ حساباً جديداً."},
  {roots: ["مرحباً", "أهلاً"], reply: "وعليكم السلام ورحمة الله وبركاته! أنا مساعدك الذكي Alislamiah-AI، جاهز لإجابتك عن الأسئلة والأحكام الفقهية الشرعية."},
  {roots: ["إلى اللقاء", "مع السلامة", "سلام", "باي", "وداعاً"], reply: "في أمان الله ورعايته! أتمنى أن أكون قد أفدتك، وتسعدني عودتك دائماً."},
  {roots: ["أحكام سجدتي السهو", "سجود السهو"], reply: "سجود السهو مشروع لجبر ما حصل في الصلاة من زيادة أو نقص، ويكون قبل السلام أو بعده."},
  {roots: ["أحكام سجود التلاوة", "سجدة القرآن"], reply: "يسجد القارئ والمستمع سجود التلاوة عند مروره بآية سجود، ويكبر لها دون تشهد أو تسليم."},
  {roots: ["أحكام سجود الشكر", "سجدة الشكر لله"], reply: "يسن سجود الشكر لله تعالى عند تجدد نعمة عظيمة أو اندفاع نقمة، وهو سجدة واحدة."},
  {roots: ["أحكام قصر الصلاة", "رخصة السفر", "السفر الصائم"], reply: "يشرع للمسافر قصر الصلاة الرباعية إلى ركعتين، وجمع الظهر والعصر أو المغرب والعشاء، ويجوز له الفطر في رمضان وقضاء عدد من أيام أخر."},
  {roots: ["أحكام صلاة الخوف", "كيفية صلاة الحرب"], reply: "تشرع صلاة الخوف في المعارك بكيفيات متعددة وردت في السنة النبوية لحفظ الأمن."},
  {roots: ["أحكام صلاة الجمعة", "التخلف عن الجمعة"], reply: "تجب صلاة الجمعة على كل مسلم بالغ عاقل مقيم، ومن تركها ثلاث جمع طبع الله على قلبه."},
  {roots: ["أحكام صلاة العيدين", "تكبيرات العيد"], reply: "صلاة العيدين سنة مؤكدة، ويُسن فيها التكبير الزائد والخطبة بعدها لإدخال الفرح."},
  {roots: ["أحكام صلاة الاستسقاء", "طلب الغيث"], reply: "تشرع صلاة الاستسقاء جماعة في المصلى عند احتضار المطر وتأخر الغيث مع التذلل والافتقار."},
  {roots: ["أحكام صلاة الكسوف والخسوف", "الفزع إلى الصلاة"], reply: "تستحب صلاة الكسوف والخسوف بركعتين في كل ركعة قيامان وركوعان وسجودان."},
  {roots: ["أحكام الجنازة"], reply: "صلاة الجنازة فرض كفاية، وأركانها أربع تكبيرات تقرأ فيها الفاتحة والصلاة والدعاء."},
  {roots: ["أحكام التغسيل"], reply: "تغسيل الميت وتكفينه والصلاة عليه ودفنه فروض كفاية على المجتمع المسلم."},
  {roots: ["أحكام الدفن"], reply: "يُسن دفن الميت في لحد مقبرة المسلمين موجهاً لقبلة الكعبة على شقه الأيمن."},
  {roots: ["أحكام التعزية"], reply: "تشرع التعزية لأهل الميت لتسلية مصيبتهم ودعوتهم للصبر والاحتساب دون إحداث مآتم."},
  {roots: ["أحكام القبور"], reply: "تشرع زيارة القبور للرجال والنساء للاتعاظ بالآخرة والدعاء للموتى بالسلام والمغفرة."},
  {roots: ["أحكام المفطرات"], reply: "يبطل الصيام بالأكل والشرب عمداً والجماع والاستقاءة العمد ونزول دم الحيض والنفاس."},
  {roots: ["أحكام القضاء"], reply: "يجب قضاء الأيام المفطرة من رمضان قبل حلول رمضان التالي، ويجوز التفريق والتتابع."},
  {roots: ["أحكام الفدية"], reply: "من عجز عن الصيام لكبر أو مرض مزمن لزمته الفدية بإطعام مسكين عن كل يوم أفطره."},
  {roots: ["أحكام الحامل"], reply: "الحامل والمرضع إن خافتا على ولديهما أفطرتتا وقاضتا، وبعضهم أوجب مع القضاء الإطعام."},
  {roots: ["أحكام التطوع"], reply: "أفضل الصيام تطوعاً صيام يوم وإفطار يوم، وصيام الإثنين والخميس وثلاثة أيام من كل شهر."},
  {roots: ["أحكام الأيام البيض"], reply: "يُسن صيام الأيام البيض وهي الثالث عشر والرابع عشر والخامس عشر من كل شهر قَمَري."},
  {roots: ["أحكام زكاة الفطر"], reply: "تجب زكاة الفطر على كل مسلم يملِك قوت يومه، ومقدارها صاع من تمر أو شعير أو طعام."},
  {roots: ["أحكام زكاة المال", "التجارة المالية"], reply: "تجب زكاة المال إذا بلغ النصاب الشرعي وحال عليه الحول القمري، وتقوم عروض التجارة بسعر السوق وقت الحول."},
  {roots: ["أحكام زكاة النقدين"], reply: "تجب الزكاة في الذهب والفضة والأوراق النقدية بنسبة ربع العشر (2.5 بالمئة)."},
  {roots: ["أحكام الزروع", "الحبوب"], reply: "تجب زكاة الزروع والثمار فيما يكطال ويدخر إذا بلغ خمسة أوسق، ومقدارها العشر أو نصفه."},
  {roots: ["أحكام المعادن", "الركاز"], reply: "الركاز المستخرج من دفن الجاهلية فيه الخمس مباشرة، والمعادن فيها الزكاة بشروطها."},
  {roots: ["أحكام المصارف"], reply: "تُصرف الزكاة للأصناف الثمانية المذكورة في سورة التوبة كالفقراء والمساكين."},
  {roots: ["أحكام الحج"], reply: "الحج فرض على كل مسلم مستطيع في عمره مرة، وأركانه الإحرام والطواف والسعي والوقوف بعرفة."},
  {roots: ["أحكام العمرة"], reply: "العمرة سنة مؤكدة أو واجبة في العمر مرة، وتكفر ما بينها وبين العمرة الأخرى لمن أتمها."},
  {roots: ["أحكام الإحرام"], reply: "يجب الإحرام من الميقات المحدد لمن أراد الحج أو العمرة، ومن جاوزه بلا إحرام لزمه دم."},
  {roots: ["أحكام المحظورات"], reply: "يحرم على المحرم لبس المخيط للرجل، وتغطية الرأس، وقص الأظافر، وحلق الشعر، والطيب."},
  {roots: ["أحكام الطواف"], reply: "طواف الإفاضة ركن من أركان الحج، وطواف الوداع واجب على الآفاقي قبل مغادرة مكة."},
  {roots: ["أحكام السعي"], reply: "يبدأ السعي من الصفا وينتهي بالمروة سبعة أشواط تامة بعد طواف صحيح في الحج أو العمرة."},
  {roots: ["أحكام الوقوف"], reply: "الوقوف بعرفة ركن الحج الأعظم، والمبيت بمزدلفة ومِنا من واجبات الحج اللازمة."},
  {roots: ["أحكام الجمرات"], reply: "يجب رمي الجمرات الثلاث أيام التشريق حصاة حصاة مع التكبير لقول النبي صلى الله عليه وسلم."},
  {roots: ["أحكام الأضحية"], reply: "الأضحية سنة مؤكدة لمن استطاع، ويشترط فيها السلامة والسن المعتبرة في الأنعام."},
  {roots: ["أحكام العقيقة"], reply: "العقيقة سنة مؤكدة تذبح عن المولود يوم سابعه، شاتان عن الغلام وشاة عن الجارية."},
  {roots: ["أحكام البيع"], reply: "يقوم البيع على الإيجاب والقبول ورضا المتبايعين، وخلوه من الربا والغرر المحرم."},
  {roots: ["أحكام البيوع"], reply: "تحرم بيوع الغرر والمجهول والمعدوم والربا بأنواعه لما تسبب من أكل أموال الناس بالباطل."},
  {roots: ["أحكام الإجارة"], reply: "الإجارة عقد جائز على المنافع أو الأعمال بأعمال معلومة وأجور محددة غير مجهولة."},
  {roots: ["أحكام الشركة"], reply: "الشركة والمضاربة جائزتان لتنمية الأموال بالشروط الشرعية وتوزيع الأرباح بالاتفاق."},
  {roots: ["أحكام الرهن"], reply: "الرهن وثيقة بالدين لاستيفائه عند التعذر، والكفالة التزام بالنفس والضمان بالمال."},
  {roots: ["أحكام الوكالة"], reply: "تجوز الوكالة في كل حق يقبل النيابة كالبيع والشراء والخصومة وقبض الديون."},
  {roots: ["أحكام اللقطة"], reply: "اللقطة تُعَرّف سنة كاملة، واللقيط نفس معصومة يجب رعايتها والإنفاق عليها."},
  {roots: ["أحكام الوقف"], reply: "الوقف قربة عظيمة يحبس فيها الواقف أصل المال ويسپل منفعته في وجوه الخير والبر."},
  {roots: ["أحكام الميراث"], reply: "الميراث نظام عادل يوزع التركة على أصحاب الفروض والعصبات بحسب الأنصباء القرآنية."},
  {roots: ["أحكام الوصية"], reply: "تجوز الوصية لغير الوارث بحدود ثلث التركة فقط، ولا وصية لوارث إلا بإجازة الورثة."},
  {roots: ["أحكام النكاح"], reply: "يقوم النكاح الصحيح على الإيجاب والقبول، ووجود الولي، والشهود، وخلو الزوجين من الموانع."},
  {roots: ["أحكام الطلاق"], reply: "الخلع فرقة بعوض تأخذه الزوجة لتباري به زوجها، والطلاق بائن والعدة تحصين للرحم."},
  {roots: ["أحكام الرضاع"], reply: "يحرم من الرضاع ما يحرم من النسب بشرط خمس رضعات مشبعات في سن الحولين الأولين."},
  {roots: ["أحكام الحضانة"], reply: "الأم أحق بحضانة طفلها ما لم تتزوج بأجنبي، وحقها يسقط بالزواج أو بوجود مانع شرعي."},
  {roots: ["أحكام الجنايات"], reply: "الجنايات توجب القصاص في العمد، والدية والكفارة في الخطأ لحفظ دماء البشر."},
  {roots: ["أحكام الحدود"], reply: "الحدود عقوبات مقدرة شرعاً لحفظ الدين والأعراض والأموال كحد السرقة والزنا."},
  {roots: ["أحكام التعزير"], reply: "التعزير عقوبات غير مقدرة شرعاً يجتهد فيها ولي الأمر والقاضي لدفع الجرائم وحماية المجتمع."},
  {roots: ["أحكام الجهاد"], reply: "يُشرع الجهاد للدفاع عن الدين، وتجوز الهدنة والعهود مع الكفار إذا وجدت مصلحة راجحة."},
  {roots: ["أحكام الذمة"], reply: "أهل الذمة لهم ما للمسلمين من حقوق الحماية والرعاية مقابل الجزية وحفظ النظام."},
  {roots: ["أحكام الآداب"], reply: "أمر الإسلام بححسن الخلق، والصدق، والأمانة، والوفاء، وحرم الكذب والغيبة والنميمة."},
  {roots: ["أحكام الصلة"], reply: "بر الوالدين وصلة الرحم من أعظم القربات الموجبة للجنة، وعقوقهما من أكبر الكبائر."},
  {roots: ["أحكام الجوار"], reply: "يجب كف الأذى عن الجار وإكرامه، وإكرام الضيف من خصال الإيمان والتقوى لله تعالى."},
  {roots: ["أحكام البيئة"], reply: "أمر الإسلام بالإحسان للحيوان وحرم تعذيبه، وحث على غرس الأشجار وإماطة الأذى."},
  {roots: ["أحكام التوكل"], reply: "التوكل الحق يجمع بين صدق الاعتماد على الله وفعل الأسباب، والرضا بالقدر يورث الطمأنينة."},
  {roots: ["أحكام الإخلاص"], reply: "الإخلاص شرط لقبول الأعمال، والمراقبة استشعار نظر الله في السر والعلن دائماً وأبداً."},
  {roots: ["أحكام التوبة"], reply: "التوبة تجب ما قبلها، وشرائطها: الإقلاع عن الذنب، والندم، والعزم على عدم العودة."},
  {roots: ["أحكام الذكر"], reply: "ذكر الله يجلو القلوب، والاستغفار يفتح الأقفال، والدعاء مخ العبادة وأعظم أسباب الإجابة."},
  {roots: ["أحكام القيام"], reply: "قيام الليل دأب الصالحين ومطردة للداء عن الجسد، وأفضلها صلاة جوف الليل الآخر."},
  {roots: ["أحكام التطوع المطلق"], reply: "يُسن صيام التطوع في الأيام الفاضلة كعاشوراء وعرفة وست شوال والاثنين والخميس."},
  {roots: ["أحكام الصدقة"], reply: "الصدقة الخفية تطفئ غضب الرب، وتظل صاحبها في ظل عرشه يوم لا ظل إلا ظله."},
  {roots: ["أحكام الإصلاح"], reply: "إصلاح ذات البين أفضل من درجة الصيام والصلاة، وفساد ذات البين هي الحالقة."},
  {roots: ["أحكام اليتامى"], reply: "كافل اليتيم رفيق النبي صلى الله عليه وسلم في الجنة كهاتين وأشار بإصبعيه."},
  {roots: ["أحكام اللسان"], reply: "حفظ اللسان من الغيبة والنميمة والبهتان من أعظم وسائل النجاة من عذاب النار."},
  {roots: ["أحكام الستر"], reply: "من ستر مسلماً في الدنيا ستره الله في الدنيا والآخرة، والجزاء من جنس العمل."},
  {roots: ["أحكام الظلم"], reply: "اتقوا الظلم فإن الظلم ظلمات يوم القيامة، وادعوا لنصرة المظلوم وردع الظالم."},
  {roots: ["أحكام الحسد"], reply: "إياكم والحسد فإن الحسد يأكل الحسنات كما تأكل النار الحطب اليابس."},
  {roots: ["أحكام الكبر"], reply: "الكبر بطر الحق وغمط الناس، وهو مانع من قبول الحق ودخول الجنة مع الأتقياء."},
  {roots: ["أحكام الغيبة"], reply: "الغيبة والنميمة من الكبائر المحرمة التي تفسد المجتمعات وتورث عذاب القبر."},
  {roots: ["أحكام الرشوة"], reply: "الرشوة والربا من أكبر الكبائر الموبقة المهلكة لصاحبها في الدنيا والآخرة."},
  {roots: ["أحكام السحر"], reply: "من أتى كاهناً أو عرافاً فصدقه بما يقول فقد كفر بما أنزل على محمد صلى الله عليه وسلم."},
  {roots: ["أحكام الشهادة"], reply: "شهادة الزور من أكبر الكبائر المهلكة بعد الإشراك بالله وعقوق الوالدين."},
  {roots: ["أحكام اليمين"], reply: "اليمين الغموس هي التي يقتطع بها مال امرئ مسلم بغير حق، وتغمس صاحبها بالنار."},
  {roots: ["أحكام القطيعة"], reply: "لا يدخل الجنة قاطع رحم لحديث البخاري الصحيح، وصلتها تزيد في الأجل والرزق."},
  {roots: ["أحكام العقوق"], reply: "عقوق الوالدين من أكبر الكبائر المهلكة، وبرهما من أحب الأعمال إلى الله تعالى."},
  {roots: ["أحكام التشبه"], reply: "لعن النبي صلى الله عليه وسلم المتشبهين من الرجال بالنساء والمتشبهات بالرجال."},
  {roots: ["أحكام النياحة"], reply: "النياحة ورفع الصوت بالويل والثبور على الميت من أعمال الجاهلية المحرمة."},
  {roots: ["أحكام الهجر"], reply: "لا يحل لمسلم أن يهجر أخاه فوق ثلاث ليال وخيرهما يبدأ بالسلام."},
  {roots: ["أحكام الغضب"], reply: "ليس الشديد بالصرعة، بل الشديد الذي يملك نفسه عند الغضب، ولا تغضب ولك الجنة."},
  {roots: ["أحكام الرياء"], reply: "أخوف ما أخاف عليكم الشرك الأصغر وهو الرياء الذي يحبط صالح الأعمال يوم القيامة."},
  {roots: ["أحكام المعروف"], reply: "تغيير المنكر باليد أو اللسان أو القلب من أصول الدين وإصلاح المجتمع المسلم."},
  {roots: ["أحكام النصيحة"], reply: "الدين النصيحة لله ولكتابه ولرسوله ولأئمة المسلمين وعامتهم بطلب الخير لهم."},
  {roots: ["أحكام الوفاء"], reply: "الوفاء بالعهد وحفظ ود الأصدقاء وصلة المودة من شيم الكرام الأبرار الصالحين."},
  {roots: ["أحكام الضيافة"], reply: "إكرام الضيف يوم وليلة، والضيافة ثلاثة أيام، وما زاد فهو صدقة تؤجر عليها."},
  {roots: ["أحكام صلة الرحم"], reply: "ليس الواصل بالمكافئ، بل الواصل الذي إذا قطعت رحمه وصلها بالعفو والصفح."},
  {roots: ["أحكام الخدم"], reply: "الرفق بالخدم والمماليك وإطعامهم مما تأكلون وتلبسون من توجيهات الإسلام السامية."},
  {roots: ["أحكام التوقير"], reply: "توقير الكبير ورحمة الصغير من آداب الإسلام الرفيعة لحفظ تماسك المجتمع وأخلاقه."},
  {roots: ["أحكام الحلم"], reply: "الحلم والأناة صفات نبيلة يحبها الله، والتعجل من الشيطان في تصريف الأمور."},
  {roots: ["أحكام الصبر"], reply: "الصبر الجميل هو الذي لا شكوى فيه لغير الله، والرضا التام بالقدر المقدر بحكمة."},
  {roots: ["أحكام الرضا"], reply: "عجباً لأمر المؤمن إن أمره كله خير، إن أصابته سرّاء شكر فكان خيراً له."}
];
async function processMessage(text){
  if(!text)text=userInput.value.trim();if(!text)return;
  const check=canSendMessage();if(!check.allowed){appendMessage("<b>انتهت رسائلك</b>","blocked",false);return;}
  triggerFullGlow();
  const replyLang=detectLang(text);
  const lower=text.toLowerCase();
  if(lower.startsWith("صورة:")||lower.startsWith("image:")){let p=text.replace(/صورة:|image:/i,"").trim();appendMessage(text,"user");userInput.value="";incrementCount();updateLimitUI();updateSendIcon();const url=generateImagePollinations(p||"mosque");appendMessage("", "ai", true, "Pollinations AI", `<div>🎨 ${p}</div><img src="${url}" style="width:100%;border-radius:12px;margin-top:8px" onclick="openEditor(this.src)">`);return;}
  if(lower.startsWith("فيديو:")||lower.startsWith("video:")){let p=text.replace(/فيديو:|video:/i,"").trim();appendMessage(text,"user");userInput.value="";incrementCount();updateLimitUI();updateSendIcon();appendMessage("🎬 جاري إنشاء الفيديو...","ai",false);const vurl=await generateVideoCanvas(p||"ocean");const html=`<div>🎬 ${p}</div><video src="${vurl}" controls style="width:100%;border-radius:12px;margin-top:8px"></video>`;appendMessage("", "ai", true, "Canvas Video", html);return;}
  if(lower.includes("حل")||text.match(/[0-9x+\-*/^=]/)){const mr=solveMathPro(text);if(mr){appendMessage(text,"user");userInput.value="";incrementCount();updateLimitUI();updateSendIcon();appendMessage(mr,"ai",true,"Math.js");return;}}
  appendMessage(text,"user");userInput.value="";incrementCount();updateLimitUI();updateSendIcon();
  let reply=null,source=null;
  const codingRoots = ["كود", "كيف أصمم", "برمجة", "خطأ", "html", "css", "javascript", "دالة", "تصحيح", "script", "code", "bug", "function", "python", "java"];
  const isCodingQuery = codingRoots.some(root => lower.includes(root));
  if(!isCodingQuery){
    for(const item of chatKnowledge){
      for(const root of item.roots){
        if(lower.includes(root.toLowerCase()) || root.toLowerCase().includes(lower.trim())){
          reply=item.reply;source="Gemalot DB - قاعدة بيانات محلية";break;
        }
      }
      if(reply)break;
    }
  }
  if(reply && !isCodingQuery){
    if(replyLang==='fr' && text.includes("الوضوء")) reply="Les annulations des ablutions: sortie des deux voies, perte de raison, etc.";
    if(replyLang==='en' && text.includes("الوضوء")) reply="Nullifiers of wudu: excretion, loss of reason, touching private parts with desire, etc.";
    appendMessage(reply,"ai",true,source);if(lastVoiceInput){speakText(reply);lastVoiceInput=false;}return;
  }
  appendMessage(t.searching,"ai",false);
  let result=null;
  let resultSource=null;
  if(isCodingQuery){
    result = await fetchGemalotMultiSource(text);
    if(result) resultSource = "مصادر تقنية ومفتوحة متعددة (MDN/DuckDuckGo) - دالة برمجة";
  }
  if(!result){
    const web=await searchAllSources(text,replyLang);
    if(web){
      result = web.text;
      resultSource = web.source;
    }
  }
  const last=chatContainer.lastChild;if(last&&last.textContent===t.searching)last.remove();
  if(result){
    appendMessage(result,"ai",true,resultSource||"Eisen.translastor");
    if(lastVoiceInput){speakText(result);lastVoiceInput=false;}
  } else {
    appendMessage(t.noAnswer,"ai",true,"لا يوجد");
  }
}
function sendSuggestion(txt){processMessage(txt);}
function toggleTools(){const pop=document.getElementById('toolsPopup');if(pop)pop.classList.toggle('open');}
function closeTools(){const pop=document.getElementById('toolsPopup');if(pop)pop.classList.remove('open');}
document.addEventListener('click',e=>{const pop=document.getElementById('toolsPopup');const plus=document.getElementById('plusBtn');if(pop&&plus&&!pop.contains(e.target)&&e.target!==plus)pop.classList.remove('open');});
function updateSendIcon(){
  const val=userInput.value.trim();
  const icon=document.getElementById('sendIcon');const mic=document.getElementById('micBtn');const btn=document.getElementById('sendBtn');
  if(val.length>0){icon.textContent='↑';btn.classList.remove('wave');btn.classList.add('active');if(mic)mic.style.display='none';}
  else{icon.textContent='◍';btn.classList.remove('active');btn.classList.add('wave');if(mic)mic.style.display='flex';}
}
function updatePlaceholder(){if(!userInput)return;userInput.placeholder=t.placeholder||"اسأل Gemalot";}
let lastVoiceInput=false;
function speakText(text){
  if(!('speechSynthesis' in window)) return;
  window.speechSynthesis.cancel();
  let clean=text.replace(/<[^>]*>/g,'').slice(0,300);
  const utter=new SpeechSynthesisUtterance(clean);
  utter.lang=currentLang==='ar'?'ar-SA':currentLang==='fr'?'fr-FR':'en-US';
  window.speechSynthesis.speak(utter);
}
let recognition=null;
function toggleMic(){
  if(!('webkitSpeechRecognition' in window)&&!('SpeechRecognition' in window)){alert('المتصفح لا يدعم المايك');return;}
  const SR=window.SpeechRecognition||window.webkitSpeechRecognition;
  if(recognition){recognition.stop();recognition=null;const mb=document.getElementById('micBtn');if(mb)mb.textContent='🎙️';return;}
  recognition=new SR();recognition.lang=currentLang==='ar'?'ar-SA':currentLang==='fr'?'fr-FR':'en-US';recognition.interimResults=false;
  recognition.start();lastVoiceInput=true;
  const mb=document.getElementById('micBtn');if(mb)mb.textContent='🔴';
  recognition.onresult=e=>{
    const transcript=e.results[0][0].transcript;
    userInput.value=transcript;updateSendIcon();setTimeout(()=>processMessage(transcript),400);
  };
  recognition.onend=()=>{recognition=null;const mb=document.getElementById('micBtn');if(mb)mb.textContent='🎙️';};
}
if(userInput){
  userInput.addEventListener("focus",()=>{triggerFullGlow();closeTools();});
  userInput.addEventListener("input",updateSendIcon);
  userInput.addEventListener("keydown",e=>{if(e.key==="Enter"&&!e.shiftKey){e.preventDefault();processMessage();}});
}
if(sendBtn)sendBtn.addEventListener("click",()=>processMessage());

applyLanguage(currentLang);renderChatList();updateLimitUI();renderSuggestions();updatePlaceholder();updateSendIcon();setInterval(updateLimitUI,60000);
// Restore selected model from localStorage
(function restoreModel(){
  var saved = localStorage.getItem('gemalot_current_model') || 'normal';
  if(saved !== 'normal' && saved !== 'pro') saved = 'normal';
  // slight delay so DOM is fully ready
  setTimeout(function(){ selectModelFromDropdown(saved); }, 50);
})();
let currentImage=null;const editModal=document.getElementById("editModal");const editCanvas=document.getElementById("editCanvas");const ctx=editCanvas?editCanvas.getContext("2d"):null;
function openEditor(src){if(!editModal||!editCanvas)return;editModal.style.display="flex";currentImage=new Image();currentImage.crossOrigin="anonymous";currentImage.onload=()=>{editCanvas.width=currentImage.width;editCanvas.height=currentImage.height;ctx.drawImage(currentImage,0,0);};currentImage.src=src;}
function closeEditor(){if(editModal)editModal.style.display="none";}
function applyFilter(type){if(!ctx||!editCanvas)return;const imageData=ctx.getImageData(0,0,editCanvas.width,editCanvas.height);const data=imageData.data;for(let i=0;i<data.length;i+=4){if(type==="grayscale"){const avg=(data[i]+data[i+1]+data[i+2])/3;data[i]=data[i+1]=data[i+2]=avg;}else if(type==="sepia"){data[i]=Math.min(255,data[i]*0.393+data[i+1]*0.769+data[i+2]*0.189);data[i+1]=Math.min(255,data[i]*0.349+data[i+1]*0.686+data[i+2]*0.168);data[i+2]=Math.min(255,data[i]*0.272+data[i+1]*0.534+data[i+2]*0.131);}else if(type==="invert"){data[i]=255-data[i];data[i+1]=255-data[i+1];data[i+2]=255-data[i+2];}else if(type==="bright"){data[i]=Math.min(255,data[i]+30);data[i+1]=Math.min(255,data[i+1]+30);data[i+2]=Math.min(255,data[i+2]+30);}}ctx.putImageData(imageData,0,0);}
function rotateCanvas(deg){if(!editCanvas||!ctx)return;const tmp=document.createElement("canvas");tmp.width=editCanvas.height;tmp.height=editCanvas.width;const tctx=tmp.getContext("2d");tctx.translate(tmp.width/2,tmp.height/2);tctx.rotate(deg*Math.PI/180);tctx.drawImage(editCanvas,-editCanvas.width/2,-editCanvas.height/2);editCanvas.width=tmp.width;editCanvas.height=tmp.height;ctx.drawImage(tmp,0,0);}
function addTextOverlay(){const txt=prompt("اكتب النص:");if(!txt||!ctx)return;ctx.fillStyle="#facc15";ctx.font="bold 40px Arial";ctx.fillText(txt,30,50);}
function downloadCanvas(){if(!editCanvas)return;const link=document.createElement("a");link.download="gemalot_edited.png";link.href=editCanvas.toDataURL();link.click();}
</script>
</body>
</html>
