<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
<meta charset="UTF-8"><meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Gemalot</title>
<script src="https://cdnjs.cloudflare.com/ajax/libs/mathjs/11.11.0/math.min.js"></script>
<style>
/* ===== Splash - ستايلك الأصلي ===== */
.ai-splash-screen{position:fixed;inset:0;background:#07111f;display:flex;flex-direction:column;justify-content:center;align-items:center;z-index:99999;overflow:hidden;perspective:1000px;transition:opacity .8s cubic-bezier(0.4,0,0.2,1),visibility .8s ease}
.ai-splash-screen.hidden{opacity:0;visibility:hidden;pointer-events:none}
.ai-splash-glow{position:absolute;width:280px;height:280px;background:radial-gradient(circle,rgba(8,127,255,.35) 0%,rgba(0,195,255,.12) 50%,transparent 75%);border-radius:50%;animation:bgGlow 3s ease-in-out infinite alternate}
.ai-icon-container{position:relative;z-index:2;display:flex;justify-content:center;align-items:center;transform-style:preserve-3d;animation:flipAndScale 2s cubic-bezier(0.34,1.56,0.64,1) forwards}
.ai-splash-icon{width:110px;height:110px;border-radius:26px;object-fit:cover;box-shadow:0 12px 35px rgba(8,127,255,.45),0 0 20px rgba(0,195,255,.25);animation:floatingGlow 2.5s ease-in-out 2s infinite alternate}
.ai-splash-text{position:relative;z-index:2;margin-top:30px;font-size:1.3rem;font-weight:700;color:#f0f7ff;letter-spacing:.5px;opacity:0;transform:translateY(18px);animation:textAppear .8s ease-out 1.5s forwards;text-shadow:0 4px 18px rgba(0,0,0,.7),0 0 12px rgba(8,127,255,.3)}
@keyframes flipAndScale{0%{opacity:0;transform:scale(.2) rotateY(180deg) rotateX(45deg) translateY(100px)}50%{opacity:1;transform:scale(1.25) rotateY(360deg) rotateX(-15deg) translateY(-15px)}75%{transform:scale(.95) rotateY(540deg) rotateX(5deg) translateY(5px)}100%{opacity:1;transform:scale(1) rotateY(720deg) rotateX(0) translateY(0)}}
@keyframes floatingGlow{0%{transform:scale(1)}100%{transform:scale(1.05);box-shadow:0 18px 45px rgba(8,127,255,.65),0 0 30px rgba(0,195,255,.5)}}
@keyframes bgGlow{0%{transform:scale(.85);opacity:.5}100%{transform:scale(1.2);opacity:.9}}
@keyframes textAppear{to{opacity:1;transform:translateY(0)}}

/* ===== App - مطابق للفيديو ===== */
*{box-sizing:border-box;margin:0;padding:0}
body{background:#0b0f19;color:#fff;font-family:Arial,sans-serif;min-height:100vh;display:flex;flex-direction:column;overflow-x:hidden}
.header{height:56px;background:linear-gradient(90deg,#facc15,#f97316,#22c55e,#3b82f6);display:flex;align-items:center;justify-content:space-between;padding:0 12px;position:fixed;top:0;left:0;width:100%;z-index:1000}
.header-title{font-size:18px;font-weight:bold;color:#fff}
.header-actions{display:flex;gap:6px;align-items:center}
.menu-btn{width:36px;height:36px;background:rgba(0,0,0,.25);border:none;border-radius:10px;cursor:pointer;display:flex;flex-direction:column;justify-content:center;align-items:center;gap:4px;color:#fff;font-size:16px}
.menu-btn span{display:block;width:18px;height:2.5px;background:#fff;border-radius:2px}
.counter-badge{background:rgba(0,0,0,.4);padding:5px 10px;border-radius:12px;font-size:11px;color:#facc15;font-weight:bold;border:1px solid rgba(250,204,21,.3)}
.models-bar{position:fixed;top:56px;left:0;width:100%;display:flex;gap:8px;padding:10px 12px;overflow-x:auto;background:#0b0f19;border-bottom:1px solid #1f2937;z-index:950;scrollbar-width:none}
.model-btn{flex-shrink:0;padding:8px 14px;border-radius:20px;border:1.5px solid rgba(255,255,255,.15);background:transparent;color:#e5e7eb;font-size:11px;font-weight:600;cursor:pointer;white-space:nowrap}
.model-btn.active{background:linear-gradient(90deg,#facc15,#f97316,#22c55e,#3b82f6);color:#111;border-color:transparent}
.model-btn.locked{opacity:.8}
.sidebar{position:fixed;top:108px;left:0;width:280px;height:calc(100vh - 108px);background:#111827;border-right:1px solid #1f2937;z-index:900;transform:translateX(-100%);transition:transform .3s;overflow-y:auto;padding:14px}
.sidebar.open{transform:translateX(0)}
.sidebar h3{font-size:14px;margin-bottom:10px;color:#facc15}
.chat-item{display:flex;align-items:center;justify-content:space-between;padding:10px;border-radius:10px;background:#1f2937;margin-bottom:6px;font-size:13px}
.chat-item .chat-title{flex:1;cursor:pointer;overflow:hidden;text-overflow:ellipsis;white-space:nowrap}
.delete-btn{background:none;border:none;color:#ef4444;font-size:15px;cursor:pointer}
.new-chat-btn{width:100%;padding:10px;border:none;border-radius:10px;background:linear-gradient(90deg,#facc15,#f97316);color:#111;font-weight:bold;cursor:pointer;margin-bottom:12px}
.chat-container{flex:1;width:100%;max-width:800px;margin:108px auto 180px;padding:16px;display:flex;flex-direction:column}
.welcome-box{text-align:center;margin:auto;padding:40px 0}
.welcome-box h1{font-size:28px;margin-bottom:8px}
.welcome-box .line{width:40px;height:2px;background:#4b5563;margin:0 auto 10px}
.welcome-box p{color:#9ca3af;font-size:14px}
.limit-info{margin-top:16px;padding:12px;background:#111827;border-radius:12px;border:1px solid #1f2937;font-size:11px;line-height:1.6;color:#9ca3af}
.message{max-width:85%;padding:12px 14px;margin:8px 0;border-radius:16px;font-size:14px;line-height:1.7;word-wrap:break-word;white-space:pre-wrap}
.message.user{background:linear-gradient(90deg,#facc15,#f97316,#22c55e,#3b82f6);color:#fff;font-weight:bold;margin-right:auto;border-bottom-left-radius:4px}
.message.ai{background:#fff;color:#111;margin-left:auto;border-bottom-right-radius:4px;box-shadow:0 2px 6px rgba(0,0,0,.1)}
.message.ai img,.message.ai video{max-width:100%;border-radius:12px;margin-top:8px;display:block}
.message.blocked{background:#7f1d1d;color:#fff;margin:0 auto;max-width:95%;text-align:center;border-radius:12px}
.source-tag{font-size:9px;color:#6b7280;margin-top:6px;display:block;border-top:1px solid #e5e7eb;padding-top:4px}
.suggestions{position:fixed;bottom:86px;left:0;width:100%;display:flex;gap:8px;padding:8px 12px;overflow-x:auto;background:transparent;z-index:999;scrollbar-width:none}
.sug-btn{flex-shrink:0;padding:8px 14px;border-radius:20px;border:1px solid rgba(255,255,255,.15);background:rgba(255,255,255,.08);color:#e5e7eb;font-size:11px;cursor:pointer;backdrop-filter:blur(6px)}
.sug-btn:hover{background:rgba(250,204,21,.15);border-color:#facc15}
.input-area{position:fixed;bottom:0;left:0;width:100%;padding:10px 12px 14px;background:linear-gradient(to top,#0b0f19 75%,transparent);z-index:1000}
.gemini-bar{max-width:720px;margin:0 auto;display:flex;align-items:center;background:#ffffff;border-radius:32px;padding:6px 8px;gap:6px;box-shadow:0 8px 30px rgba(0,0,0,.35),0 0 0 1px rgba(0,0,0,.05);transition:box-shadow .3s}
.gemini-bar:focus-within{box-shadow:0 10px 36px rgba(0,0,0,.45),0 0 0 2px rgba(250,204,21,.5)}
.plus-btn{width:40px;height:40px;border-radius:50%;border:none;background:#fff;color:#111;font-size:26px;cursor:pointer;display:flex;align-items:center;justify-content:center}
.input-wrapper{flex:1;display:flex;align-items:center;min-width:0}
.input-wrapper textarea{flex:1;border:none;outline:none;resize:none;font-size:15px;color:#111;background:transparent;max-height:90px;padding:10px 2px;line-height:1.4}
.input-wrapper textarea::placeholder{color:#6b7280}
.mic-btn{width:40px;height:40px;border-radius:50%;border:none;background:transparent;color:#111;font-size:20px;cursor:pointer;display:flex;align-items:center;justify-content:center}
.send-btn{width:44px;height:44px;border-radius:50%;border:none;cursor:pointer;display:flex;align-items:center;justify-content:center;transition:.2s}
.send-btn.wave{background:#e0f2fe;color:#111}
.send-btn.active{background:linear-gradient(135deg,#facc15,#f97316,#22c55e,#3b82f6);color:#111}
.tools-popup{position:fixed;bottom:90px;left:50%;transform:translateX(-50%);max-width:340px;width:90%;background:#111827;border:1px solid #1f2937;border-radius:18px;padding:10px;display:none;z-index:1001;box-shadow:0 20px 40px rgba(0,0,0,.5)}
.tools-popup.open{display:block}
.tools-popup button{width:100%;text-align:right;padding:10px 12px;border-radius:10px;border:none;background:transparent;color:#e5e7eb;font-size:12px;cursor:pointer;display:flex;align-items:center;gap:8px}
.tools-popup button:hover{background:#1f2937}
.full-glow{position:fixed;inset:0;pointer-events:none;z-index:9999;opacity:0;background:linear-gradient(120deg,#facc15 0%,#f97316 25%,#22c55e 50%,#3b82f6 75%,#facc15 100%);background-size:400% 400%;mix-blend-mode:screen}
.full-glow.active{animation:fullGlowFlow 1.8s ease-in-out forwards}
@keyframes fullGlowFlow{0%{background-position:0% 50%;opacity:.7}50%{background-position:80% 50%;opacity:.85}100%{background-position:0% 50%;opacity:0}}
.edit-modal{position:fixed;inset:0;background:rgba(0,0,0,.85);z-index:2000;display:none;align-items:center;justify-content:center;padding:12px}
.edit-box{background:#111827;border-radius:16px;padding:16px;width:100%;max-width:500px;max-height:90vh;overflow:auto}
.edit-box canvas{width:100%;border-radius:12px;background:#000;margin:10px 0}
.edit-tools{display:flex;flex-wrap:wrap;gap:6px}
.edit-tools button{padding:6px 10px;border-radius:8px;border:1px solid #374151;background:#1f2937;color:#fff;font-size:11px;cursor:pointer}
</style>
</head>
<body>
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

<div class="header">
  <div style="display:flex;gap:6px;align-items:center">
    <button class="menu-btn" onclick="toggleSidebar()"><span></span><span></span><span></span></button>
    <button id="settingsBtn" class="menu-btn" style="background:rgba(255,255,255,.15)">⚙️</button>
    <div id="msgCounter" class="counter-badge">25/25</div>
  </div>
  <div class="header-title">Gemalot</div>
</div>

<div class="models-bar">
  <button class="model-btn active" onclick="selectModel(this)">Gemalot Normal</button>
  <button class="model-btn locked" onclick="location.href='subscription.html'">Plus 45 🔒</button>
  <button class="model-btn locked" onclick="location.href='subscription.html'">Bronze 65 🔒</button>
  <button class="model-btn locked" onclick="location.href='subscription.html'">Silver 75 🔒</button>
  <button class="model-btn locked" onclick="location.href='subscription.html'">Gold 100 🔒</button>
</div>

<div id="sidebar" class="sidebar">
  <button class="new-chat-btn" onclick="newChat()">+ محادثة جديدة</button>
  <h3>المحادثات السابقة</h3>
  <div id="chatList"></div>
  <div style="margin-top:20px;padding:10px;background:#0b0f19;border-radius:10px;font-size:11px;color:#9ca3af">
    <div id="subStatus">الوضع: عادي (25 رسالة/يوم)</div>
    <div id="subExpiry" style="margin-top:4px"></div>
  </div>
</div>

<div id="chatContainer" class="chat-container">
  <div id="welcomeBox" class="welcome-box">
    <h1>Gemalot</h1>
    <div class="line"></div>
    <p id="welcomeText">كيف يمكنني مساعدتك؟</p>
    <div class="limit-info" id="limitInfo">متبقي: 25/25 اليوم - بدون سيرفر</div>
  </div>
</div>

<div id="suggestionsBar" class="suggestions"></div>

<div class="tools-popup" id="toolsPopup">
  <button onclick="triggerUpload('image');closeTools()"><span>🖼️</span> رفع صورة وتعديلها</button>
  <button onclick="triggerUpload('video');closeTools()"><span>🎥</span> رفع فيديو</button>
  <button onclick="askImageGen();closeTools()"><span>🎨</span> إنشاء صورة 4K</button>
  <button onclick="askVideoGen();closeTools()"><span>🎬</span> إنشاء فيديو</button>
  <button onclick="insertPrompt('حل المعادلة: ');closeTools()"><span>🧮</span> حل معادلة</button>
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
<div id="editModal" class="edit-modal"><div class="edit-box"><h3 style="color:#facc15;font-size:14px">محرر احترافي - بدون سيرفر</h3><canvas id="editCanvas"></canvas><div class="edit-tools"><button onclick="applyFilter('grayscale')">أبيض وأسود</button><button onclick="applyFilter('sepia')">Sepia</button><button onclick="applyFilter('invert')">عكسي</button><button onclick="applyFilter('bright')">تفتيح</button><button onclick="rotateCanvas(90)">دوران</button><button onclick="addTextOverlay()">نص</button><button onclick="downloadCanvas()">تحميل</button><button onclick="closeEditor()">إغلاق</button></div></div></div>

<script>
const planLimits={normal:25,plus:45,bronze:65,silver:75,gold:100};
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
function getDailyLimit(){const sub=getSubscription();if(sub&&sub.dailyLimit)return parseInt(sub.dailyLimit);if(sub&&sub.plan&&planLimits[sub.plan])return planLimits[sub.plan];return planLimits.normal;}
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
function toggleSidebar(){const sb=document.getElementById("sidebar");if(sb)sb.classList.toggle("open");}
function updateLimitUI(){checkAndResetDaily();const counter=document.getElementById("msgCounter");const subStatus=document.getElementById("subStatus");const limitInfo=document.getElementById("limitInfo");if(!counter)return;const lim=getDailyLimit();const count=parseInt(localStorage.getItem("gemalot_daily_count")||"0");const remaining=Math.max(0,lim-count);counter.textContent=`${remaining}/${lim}`;if(limitInfo)limitInfo.textContent=`متبقي: ${remaining}/${lim} اليوم - بدون سيرفر`;if(subStatus)subStatus.textContent=`الوضع: عادي (${remaining}/${lim})`;}
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
async function searchWikipediaLang(q,lang){try{let res=await safeFetch(`https://${lang}.wikipedia.org/api/rest_v1/page/summary/${encodeURIComponent(q)}`);if(res){const d=await res.json();if(d.extract&&d.extract.length>40)return {text:d.extract,source:`Wikipedia ${lang}`};}res=await safeFetch(`https://${lang}.wikipedia.org/w/api.php?action=query&list=search&srsearch=${encodeURIComponent(q)}&format=json&origin=*`);if(res){const d=await res.json();const first=d.query?.search?.[0]?.title;if(first){res=await safeFetch(`https://${lang}.wikipedia.org/api/rest_v1/page/summary/${encodeURIComponent(first)}`);if(res){const dd=await res.json();if(dd.extract)return {text:dd.extract,source:`Wikipedia ${lang}`};}}}}catch{}return null;}
async function searchAllSources(q,preferredLang){
  let langs=["ar","en","fr","es","tr","de","id","ur","fa"];
  if(preferredLang) langs=[preferredLang,...langs.filter(l=>l!==preferredLang)];
  for(let l of langs){const r=await searchWikipediaLang(q,l);if(r)return r;}
  return null;
}
const chatKnowledge=[
  {roots:["كيف يمكنك مساعدتك","مرحبا","السلام عليكم","أهلا"],reply:"وعليكم السلام! أنا Gemalot مساعدك الذكي Alislamiah-AI، جاهز لإجابتك."},
  {roots:["نواقض الوضوء"],reply:"نواقض الوضوء: الخارج من السبيلين، زوال العقل، مس الفرج بشهوة، أكل لحم الإبل، وغيرها."},
  {roots:["أحكام الصيام","الصيام"],reply:"الصيام ركن من أركان الإسلام، شروطه الإسلام والبلوغ والعقل والإقامة والصحة."},
  {roots:["أحكام سجود السهو"],reply:"سجود السهو مشروع لجبر ما حصل في الصلاة من زيادة أو نقص."},
  {roots:["Gemalot","من أنت"],reply:"Gemalot هو مساعدك الذكي، تم تطويري بواسطة شبكة Alislamiah."},

    { 
      roots: ["كيف أرسل رسالة في بريدكم", "كيف أرسل رسائل في بريد أيزن", "طريقة إرسال رسالة في بريد ايزن", "كيفية ارسال رسالة Eisen mail"], 
      reply: "لإرسال رسالة عبر البريد:\n1. اضغط على زر (جديد) الموجود في القائمة السفلية.\n2. اكتب بريد الشخص الذي تود الإرسال إليه.\n3. أضف موضوع الرسالة (العنوان أو الفكرة العامة).\n4. اكتب نص الرسالة (المحتوى الذي تريد إرساله)." 
    },

  { 
    roots: ["الوقت", "أهمية الوقت", "تعبير عن الوقت", "الوقت وأهميته", "استثمار الوقت", "استغلال الوقت"], 
    reply: "يعد الوقت من أثمن النعم التي يمتلكها الإنسان، فهو أساس النجاح والتقدم في الحياة.\n\nتكمن أهمية الوقت في أنه يساعد على تحقيق الأهداف وإنجاز الأعمال وتنظيم شؤون الحياة. ويمكن استغلاله من خلال وضع خطة يومية، وتحديد الأولويات، والابتعاد عن الملهيات، والالتزام بالمواعيد. كما أن استثمار الوقت في التعلم والعمل النافع يعود بالفائدة على الفرد والمجتمع.\n\nوفي الختام، فإن حسن استغلال الوقت هو مفتاح النجاح، لذلك يجب المحافظة عليه وعدم إضاعته فيما لا ينفع." 
  },
  { 
    roots: ["الوطن", "حب الوطن", "تعبير عن حب الوطن", "تعبير عن الوطن", "أهمية الوطن", "واجبنا نحو الوطن"], 
    reply: "الوطن ليس مجرد أرض نعيش فوقها، بل هو تاريخنا وذكرياتنا وهويتنا التي نفخر بها بين الأمم. فيه ولدنا، وعلى أرضه نشأنا، ومن خيراته نعيش ونحلم بمستقبل أفضل. لذلك فإن حب الوطن شعور فطري يسكن القلوب، ويجعل الإنسان متعلقًا بأرضه ومخلصًا لها مهما ابتعد عنها.\n\nيتجلى حب الوطن في المحافظة على ممتلكاته العامة، واحترام قوانينه، والعمل بجدٍ وإخلاص من أجل رفعته وتقدمه. فالمواطن الصالح لا يكتفي بالكلام عن حب وطنه، بل يترجمه إلى أفعال نافعة تساهم في تطوره وازدهاره. كما أن طلب العلم، ونشر الأخلاق الحسنة، ومساعدة الآخرين، كلها صور مشرقة من صور حب الوطن. وعندما يتعاون أبناء الوطن ويتحلون بروح المسؤولية، يصبح وطنهم أقوى وأكثر تقدمًا واستقرارًا.\n\nوالوطن يستحق منا الكثير، لأنه يمنحنا الأمن والانتماء والكرامة. لذلك يجب أن نحافظ عليه، وأن ندافع عنه، وأن نسعى دائمًا إلى ترك أثر طيب يساهم في بناء مستقبله المشرق للأجيال القادمة.\n\nوفي الختام، يبقى حب الوطن من أنبل المشاعر وأعظم القيم الإنسانية، فهو مصدر العزة والفخر لكل إنسان. ومن واجبنا أن نحافظ على وطننا ونخدمه بكل ما نستطيع، لأن ازدهاره هو ازدهار لنا جميعًا، ورفعته دليل على إخلاص أبنائه ووفائهم له." 
  },

    { 
      roots: ["تزخر ولاية الجزائر العاصمة", "تراث الجزائري", "معالم الجزائر العاصمة", "قصبة الجزائر", "تراث العاصمة الجزائرية"], 
      reply: "تزخر ولاية الجزائر بتراثٍ تاريخي وثقافي عريق يعكس تعاقب الحضارات التي مرت بها عبر القرون. وتعد قصبة الجزائر من أبرز معالم هذا التراث، إذ تتميز بأزقتها الضيقة ومبانيها التقليدية التي تشهد على عراقة المدينة وتاريخها العثماني. كما تحتضن الولاية العديد من المساجد القديمة والقصور والمتاحف التي تحفظ ذاكرة الجزائر وهويتها الوطنية. ويُضاف إلى ذلك التراث غير المادي المتمثل في الموسيقى الأندلسية والحرف التقليدية والعادات الاجتماعية الأصيلة التي لا تزال حاضرة في الحياة اليومية لسكان العاصمة." 
    },


  { 
    roots: ["Alislamiah AI", "ماهي Alislamiah AI", "Alislamiah bing", "الإسلامية أي أي", "شبكة الإسلامية"], 
    reply: "Alislamiah AI هو محرك البحث الذكي المطور بواسطة شبكة Alislamiah." 
  },
  { 
    roots: ["Gemalot", "Gemalot AI", "جيمايلوت", "من أنت", "من انت"], 
    reply: "Gemalot هو مساعدك الذكي الذاتي، تم تطويري بواسطة شبكة Alislamiah لخدمتك وإجابة استفساراتك." 
  },
  { 
    roots: ["من الذي صنعك", "من هو مطورك", "المدير العام لشبكة الإسلامية", "من طورك", "من صنعك"], 
    reply: "أنا نموذج ذكاء اصطناعي نصي تم تطويري بواسطة شبكة Alislamiah.\nمطوري هو المدير العام لشبكة الإسلامية الرقمية (The General Director of Alislamiah Digital Network)." 
  },
  { 
    roots: ["Eisen mail", "ماهو Eisen mail", "Alislamiah mail", "بريد ايسن", "بريد الإسلامية"], 
    reply: "Eisen Mail هو تطبيق بريد إلكتروني جديد تم تطويره بواسطة Eisen التابعة لشبكة Alislamiah." 
  },

    { 
      roots: ["التدخين", "تعبير عن التدخين", "أضرار التدخين", "مخاطر التدخين", "الإقلاع عن التدخين", "التدخين السلبي"], 
      reply: "يُعدّ التدخين من أكثر الظواهر انتشارًا في المجتمعات المعاصرة، وهو عبارة عن استنشاق الدخان الناتج عن احتراق التبغ الموجود في السجائر أو غيرها من منتجات التدخين. ويحتوي دخان التبغ على مواد كيميائية ضارة تؤثر سلبًا في صحة الإنسان، حيث تسبّب أمراضًا خطيرة مثل أمراض القلب والرئتين والسرطان. ولا تقتصر أضرار التدخين على المدخّن وحده، بل تمتد إلى الأشخاص المحيطين به فيما يُعرف بالتدخين السلبي. كما يؤدي التدخين إلى استنزاف الأموال وإضعاف القدرة البدنية والإنتاجية، لذلك تعمل الهيئات الصحية على نشر الوعي بأخطاره وتشجيع الأفراد على الإقلاع عنه حفاظًا على صحتهم وصحة المجتمع." 
    },
    { 
      roots: ["المخدرات", "آفة المخدرات", "خطر المخدرات", "أضرار المخدرات", "الإدمان", "مكافحة المخدرات"], 
      reply: "تُعتبر المخدرات من أخطر الآفات الاجتماعية التي تهدد الفرد والمجتمع، وهي مواد تؤثر على الجهاز العصبي وتُحدث تغييرًا في وظائف العقل والإدراك والسلوك. يلجأ بعض الأشخاص إلى تعاطي المخدرات بدافع الفضول أو نتيجة تأثير رفقاء السوء أو بسبب المشكلات النفسية والاجتماعية، إلا أن نتائجها غالبًا ما تكون وخيمة. فالإدمان يؤدي إلى تدهور الصحة الجسدية والعقلية، ويؤثر على التحصيل الدراسي والأداء المهني، كما قد يدفع المتعاطي إلى ارتكاب الجرائم للحصول على المال اللازم لشراء هذه المواد. لذلك تبذل الدول جهودًا كبيرة لمكافحة المخدرات من خلال التوعية والعلاج والردع القانوني، من أجل حماية الأفراد وضمان استقرار المجتمع." 
    },
    { 
      roots: ["الخمر", "أضرار الخمر", "مخاطر الخمر", "شرب الكحول", "تعبير عن الخمر", "أضرار الكحول"], 
      reply: "الخمر مشروب يحتوي على نسبة من الكحول تؤثر في العقل والسلوك عند تناوله، ويُعدّ من المشكلات الاجتماعية والصحية التي تعاني منها العديد من المجتمعات. يؤدي شرب الخمر إلى فقدان التركيز وضعف القدرة على اتخاذ القرارات السليمة، وقد يكون سببًا في وقوع حوادث المرور والمشاجرات وغيرها من السلوكات الخطيرة. كما أن الإفراط في تناوله يسبب أضرارًا صحية عديدة، خاصة على الكبد والجهاز العصبي والقلب. بالإضافة إلى ذلك، قد ينعكس تعاطي الخمر سلبًا على العلاقات الأسرية والاجتماعية، فيؤدي إلى النزاعات والتفكك الأسري. ولهذا تسعى المؤسسات التربوية والصحية إلى توعية الناس بمخاطره وتشجيعهم على تبني أساليب حياة صحية وسليمة." 
    },
    { 
      roots: ["الغش", "أضرار الغش", "مخاطر الغش", "الغش في الدراسة", "الغش التجاري", "تعبير عن الغش", "الامانة والنزاهة"], 
      reply: "الغش سلوك غير أخلاقي يقوم على الخداع والتلاعب بهدف الحصول على منفعة أو تحقيق نتيجة لا يستحقها الشخص بجهده الحقيقي. ويظهر الغش في مجالات متعددة، منها الدراسة والتجارة والعمل، حيث يؤدي إلى ضياع الحقوق وإضعاف الثقة بين أفراد المجتمع. ففي المجال الدراسي مثلًا، يمنع الغش التلميذ من اكتساب المعرفة الحقيقية ويجعله يعتمد على أساليب غير مشروعة للنجاح. أما في المعاملات التجارية، فيؤدي إلى الإضرار بالمستهلك وفقدان المصداقية. وتتمثل خطورة الغش في أنه يرسخ قيم الكسل وعدم احترام القوانين، لذلك تحرص المؤسسات التربوية والقانونية على مكافحته ونشر قيم الأمانة والنزاهة بين أفراد المجتمع." 
    },
    { 
      roots: ["الرشوة", "خطر الرشوة", "مظاهر الفساد", "أضرار الرشوة", "الشفافية والنزاهة", "تعبير عن الرشوة"], 
      reply: "تُعرّف الرشوة بأنها تقديم مال أو منفعة أو خدمة لشخص ما مقابل الحصول على امتياز أو خدمة بطريقة غير قانونية أو غير عادلة. وتُعدّ الرشوة من مظاهر الفساد التي تؤثر سلبًا في التنمية والاستقرار الاجتماعي، لأنها تؤدي إلى تفضيل المصالح الشخصية على المصلحة العامة. كما تساهم في ضياع مبدأ تكافؤ الفرص وانتشار الظلم بين المواطنين، حيث يحصل البعض على حقوق أو امتيازات بطرق غير مشروعة. وتنعكس آثار الرشوة على مختلف القطاعات، فتؤثر في الاقتصاد والإدارة والخدمات العامة. لذلك تضع الدول قوانين صارمة لمحاربتها وتشجع على ترسيخ قيم الشفافية والنزاهة واحترام القانون." 
    },
    { 
      roots: ["أطفال الشوارع", "ظاهرة أطفال الشوارع", "أسباب أطفال الشوارع", "مخاطر أطفال الشوارع", "تعبير عن أطفال الشوارع", "رعاية أطفال الشوارع"], 
      reply: "يُطلق مصطلح أطفال الشوارع على الأطفال الذين يقضون معظم أوقاتهم أو يعيشون بشكل دائم في الشوارع دون رعاية أسرية كافية أو حماية اجتماعية مناسبة. وتعود هذه الظاهرة إلى عدة أسباب، منها الفقر والتفكك الأسري وسوء الظروف المعيشية وغياب الاهتمام التربوي. ويواجه أطفال الشوارع العديد من المخاطر، مثل الاستغلال والعنف والحرمان من التعليم والرعاية الصحية، مما يؤثر في نموهم الجسدي والنفسي والاجتماعي. كما أن استمرار هذه الظاهرة يشكل تحديًا للمجتمع، لأنها تحرم الأطفال من حقوقهم الأساسية وتُصعّب عملية اندماجهم في المستقبل. ولذلك تعمل الجمعيات والمؤسسات المختصة على توفير الحماية والدعم النفسي والاجتماعي لهؤلاء الأطفال، مع السعي إلى معالجة الأسباب التي تدفعهم إلى العيش في الشوارع." 
    },


  { 
    roots: ["كيف أستخدم بريد إيزن", "كيف أستخدم أيزن مايل", "كيف أستخدم بريد الإسلامية", "كيف استخدم Eisen mail", "طريقة استخدام بريد ايزن", "ماذا أفعل لأستخدم Eisen mail"], 
    reply: "استخدام Eisen Mail كالتالي:\n1. سجل الدخول إلى حسابك (إذا لم يكن لديك حساب، قم بإنشاء حساب جديد).\n2. سيتم توجيهك إلى الواجهة الرئيسية حيث يمكنك إرسال واستقبال الرسائل والمزيد.\n\nتنبيه: يرجى الانتباه لمساحة التخزين الخاصة بك وعدم استهلاكها بالكامل لضمان استمرار الخدمة بشكل سلس." 
  }


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
  for(const item of chatKnowledge){
    for(const root of item.roots){
      if(text.toLowerCase().includes(root.toLowerCase()) || root.toLowerCase().includes(text.toLowerCase().trim())){
        reply=item.reply;source="Gemalot DB - قاعدة بيانات محلية";break;
      }
    }
    if(reply)break;
  }
  if(reply){
    // ترجمة بسيطة حسب لغة المستخدم
    if(replyLang==='fr' && text.includes("الوضوء")) reply="Les annulations des ablutions: sortie des deux voies, perte de raison, etc.";
    if(replyLang==='en' && text.includes("الوضوء")) reply="Nullifiers of wudu: excretion, loss of reason, touching private parts with desire, etc.";
    appendMessage(reply,"ai",true,source);if(lastVoiceInput){speakText(reply);lastVoiceInput=false;}return;
  }
  appendMessage(t.searching,"ai",false);
  const web=await searchAllSources(text,replyLang);
  const last=chatContainer.lastChild;if(last&&last.textContent===t.searching)last.remove();
  if(web){appendMessage(web.text,"ai",true,web.source+" (خارجي)");if(lastVoiceInput){speakText(web.text);lastVoiceInput=false;}}
  else{appendMessage(t.noAnswer,"ai",true,"لا يوجد");}
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
const settingsBtn=document.getElementById('settingsBtn');
if(settingsBtn)settingsBtn.addEventListener('click',()=>{location.href='settings.html';});

applyLanguage(currentLang);renderChatList();updateLimitUI();renderSuggestions();updatePlaceholder();updateSendIcon();setInterval(updateLimitUI,60000);
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
