<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Gemalot</title>
<style>
*{box-sizing:border-box;margin:0;padding:0}
body{background:#0b0f19;color:#fff;font-family:Arial,sans-serif;min-height:100vh;display:flex;flex-direction:column;overflow-x:hidden}
.ai-splash-screen{position:fixed;inset:0;background:#07111f;display:flex;flex-direction:column;justify-content:center;align-items:center;z-index:99999;overflow:hidden;transition:opacity .8s ease,visibility .8s ease}
.ai-splash-screen.hidden{opacity:0;visibility:hidden;pointer-events:none}
.ai-splash-glow{position:absolute;width:280px;height:280px;background:radial-gradient(circle,rgba(8,127,255,.35) 0%,rgba(0,195,255,.12) 50%,transparent 75%);border-radius:50%;animation:bgGlow 3s ease-in-out infinite alternate}
.ai-icon-container{position:relative;z-index:2;display:flex;justify-content:center;align-items:center;transform-style:preserve-3d;animation:flipAndScale 2s cubic-bezier(0.34,1.56,0.64,1) forwards}
.ai-splash-icon{width:110px;height:110px;border-radius:26px;object-fit:cover;box-shadow:0 12px 35px rgba(8,127,255,.45),0 0 20px rgba(0,195,255,.25);animation:floatingGlow 2.5s ease-in-out 2s infinite alternate}
.ai-splash-text{position:relative;z-index:2;margin-top:30px;font-size:1.3rem;font-weight:700;color:#f0f7ff;opacity:0;transform:translateY(18px);animation:textAppear .8s ease-out 1.5s forwards}
@keyframes flipAndScale{0%{opacity:0;transform:scale(.2) rotateY(180deg)}50%{opacity:1;transform:scale(1.25) rotateY(360deg)}100%{opacity:1;transform:scale(1) rotateY(720deg)}}
@keyframes floatingGlow{0%{transform:scale(1)}100%{transform:scale(1.05)}}
@keyframes bgGlow{0%{transform:scale(.85);opacity:.5}100%{transform:scale(1.2);opacity:.9}}
@keyframes textAppear{to{opacity:1;transform:translateY(0)}}
.header{height:56px;background:linear-gradient(90deg,#facc15 0%,#f97316 25%,#22c55e 50%,#3b82f6 100%);display:flex;align-items:center;justify-content:space-between;padding:0 12px;position:fixed;top:0;left:0;width:100%;z-index:1000;box-shadow:0 2px 15px rgba(0,0,0,.3)}
.menu-btn{width:42px;height:42px;background:rgba(0,0,0,.28);border:none;border-radius:12px;cursor:pointer;display:flex;flex-direction:column;justify-content:center;align-items:center;gap:4px;color:#fff}
.menu-btn span{display:block;width:18px;height:2.5px;background:#fff;border-radius:2px}
.model-current{display:flex;align-items:center;gap:6px;background:rgba(0,0,0,.35);border:none;border-radius:20px;padding:9px 14px;color:#fff;font-size:13px;font-weight:700;cursor:pointer;white-space:nowrap}
.model-dropdown{position:fixed;top:62px;left:10px;right:10px;max-width:430px;background:#fff;border-radius:20px;box-shadow:0 20px 50px rgba(0,0,0,.4);padding:10px;display:none;z-index:5000;color:#111;max-height:80vh;overflow-y:auto}
.model-dropdown.open{display:block;animation:dropIn .18s ease}
@keyframes dropIn{from{opacity:0;transform:translateY(-6px)}to{opacity:1;transform:translateY(0)}}
.model-opt{display:flex;align-items:center;justify-content:space-between;padding:14px 12px;border-radius:14px;cursor:pointer}
.model-opt.active{background:#fef3c7}
.model-divider{height:1px;background:#e5e7eb;margin:12px 0}
.sidebar{position:fixed;top:56px;left:0;width:285px;height:calc(100vh - 56px);background:#111827;border-right:1px solid #1f2937;z-index:900;transform:translateX(-100%);transition:transform .32s ease;overflow-y:auto;padding:14px}
.sidebar.open{transform:translateX(0)}
.sidebar h3{font-size:14px;margin-bottom:10px;color:#facc15}
.chat-item{display:flex;align-items:center;justify-content:space-between;padding:11px;border-radius:11px;background:#1f2937;margin-bottom:6px;font-size:13px}
.chat-item .chat-title{flex:1;cursor:pointer;overflow:hidden;text-overflow:ellipsis;white-space:nowrap}
.delete-btn{background:none;border:none;color:#ef4444;font-size:15px;cursor:pointer}
.new-chat-btn{width:100%;padding:11px;border:none;border-radius:11px;background:linear-gradient(90deg,#facc15,#f97316);color:#111;font-weight:900;cursor:pointer;margin-bottom:14px}
.chat-container{flex:1;width:100%;max-width:800px;margin:72px auto 180px;padding:16px;display:flex;flex-direction:column;min-height:60vh}
.welcome-box{text-align:center;margin:auto;padding:40px 0;animation:fadeIn .6s ease}
@keyframes fadeIn{from{opacity:0;transform:translateY(10px)}to{opacity:1;transform:translateY(0)}}
.welcome-box h1{font-size:24px;margin-bottom:8px}
.welcome-box .line{width:40px;height:2px;background:#4b5563;margin:0 auto 10px}
.welcome-box p{color:#9ca3af;font-size:13px}
.message{max-width:85%;padding:12px 14px;margin:8px 0;border-radius:16px;font-size:14px;line-height:1.6;word-wrap:break-word;white-space:pre-wrap;animation:msgIn .35s ease}
@keyframes msgIn{from{opacity:0;transform:translateY(8px)}to{opacity:1;transform:translateY(0)}}
.message.user{background:linear-gradient(90deg,#facc15,#f97316,#22c55e,#3b82f6);color:#fff;font-weight:bold;margin-right:auto;border-bottom-left-radius:4px}
.message.ai{background:#fff;color:#111;margin-left:auto;border-bottom-right-radius:4px;box-shadow:0 2px 6px rgba(0,0,0,.1)}
.suggestions{position:fixed;bottom:70px;left:0;width:100%;display:flex;gap:8px;padding:8px 12px;overflow-x:auto;background:#0b0f19;z-index:900;scrollbar-width:none}
.suggestions::-webkit-scrollbar{display:none}
.sug-btn{flex-shrink:0;padding:8px 14px;border-radius:20px;border:1px solid rgba(255,255,255,.15);background:rgba(255,255,255,.06);color:#e5e7eb;font-size:12px;cursor:pointer;white-space:nowrap}
.sug-btn:hover{background:rgba(250,204,21,.15);border-color:#facc15}
.input-area{position:fixed;bottom:0;left:0;width:100%;padding:10px 14px;background:#0b0f19;z-index:1000}
.input-wrapper{max-width:700px;margin:0 auto;display:flex;align-items:center;background:#fff;border-radius:30px;padding:4px 6px 4px 14px;box-shadow:0 2px 10px rgba(0,0,0,.2)}
.input-wrapper textarea{flex:1;border:none;outline:none;resize:none;font-size:14px;color:#111;max-height:80px;padding:8px 0;direction:rtl;background:transparent}
.send-btn{width:46px;height:46px;border-radius:50%;border:none;background:linear-gradient(90deg,#facc15,#f97316,#22c55e);color:#fff;font-size:12px;font-weight:bold;cursor:pointer;flex-shrink:0}
.full-glow{position:fixed;inset:0;pointer-events:none;z-index:9999;opacity:0;background:linear-gradient(120deg,#facc15 0%,#f97316 25%,#22c55e 50%,#3b82f6 75%,#facc15 100%);background-size:400% 400%;mix-blend-mode:screen}
.full-glow.active{animation:fullGlowFlow 2s ease-in-out forwards}
@keyframes fullGlowFlow{
0%{background-position:0% 50%;opacity:.75}
25%{background-position:40% 50%;opacity:.9}
50%{background-position:80% 50%;opacity:.8}
100%{background-position:0% 50%;opacity:0}
}
#msgCounter{position:fixed;top:60px;left:50%;transform:translateX(-50%);background:rgba(0,0,0,.6);padding:4px 12px;border-radius:20px;font-size:11px;color:#facc15;z-index:951;border:1px solid rgba(250,204,21,.4)}
</style>
</head>
<body>

<div class="ai-splash-screen" id="aiSplash">
  <div class="ai-splash-glow"></div>
  <div class="ai-icon-container"><img src="icon.png" class="ai-splash-icon" alt="Icon"></div>
  <div class="ai-splash-text">كيف يمكنني مساعدتك</div>
</div>
<script>(function(){var s=document.getElementById('aiSplash');if(!s)return;function hide(){s.classList.add('hidden');setTimeout(function(){s.style.display='none';try{s.remove()}catch(e){}},850);}setTimeout(hide,4000);s.addEventListener('click',hide);})();</script>

<div class="header">
  <div><button class="model-current" onclick="toggleModelDropdown()" id="currentModelBtn">Gemalot Normal ▼</button></div>
  <div style="display:flex;gap:8px;align-items:center">
    <button class="menu-btn" onclick="location.href='settings.html'">⚙️</button>
    <button class="menu-btn" onclick="toggleSidebar()"><span></span><span></span><span></span></button>
  </div>
</div>

<div class="model-dropdown" id="modelDropdown">
  <div class="model-opt active" data-model="normal" onclick="selectModelFromDropdown('normal')">
    <div style="flex:1;text-align:right"><div style="font-weight:700;font-size:14px">Gemalot Normal</div><small style="font-size:11px;color:#6b7280;display:block;margin-top:2px">200 رسالة - بدون Gemini - الوضع العادي</small></div><span class="check" style="color:#22c55e;font-weight:bold;font-size:18px">✓</span>
  </div>
  <div class="model-opt" data-model="pro" onclick="selectModelFromDropdown('pro')" style="margin-top:6px">
    <div style="flex:1;text-align:right"><div style="font-weight:600;font-size:14px">Gemalot Pro</div><small style="font-size:11px;color:#6b7280;display:block;margin-top:2px">15 رسالة - الوضع العادي - Gemini</small></div><span class="check" style="display:none;color:#22c55e">✓</span>
  </div>
  <div class="model-divider"></div>
  <div class="model-opt" data-model="pro_plus" onclick="goToSubscription('pro_plus')">
    <div style="flex:1;text-align:right"><div style="font-weight:600;font-size:14px">🔒 Gemalot Pro Plus</div><small style="font-size:11px;color:#6b7280;display:block;margin-top:2px">25 رسالة + مصادر أكثر + Gemini - اشتراك</small></div>
  </div>
  <div class="model-opt" data-model="pro_gold" onclick="goToSubscription('pro_gold')" style="margin-top:6px">
    <div style="flex:1;text-align:right"><div style="font-weight:600;font-size:14px">🔒 Gemalot Pro Gold</div><small style="font-size:11px;color:#6b7280;display:block;margin-top:2px">35 رسالة + فاكثر + Gemini - اشتراك</small></div>
  </div>
  <div class="model-opt" data-model="pro_extra" onclick="goToSubscription('pro_extra')" style="margin-top:6px">
    <div style="flex:1;text-align:right"><div style="font-weight:600;font-size:14px">🔒 Gemalot Pro Extra</div><small style="font-size:11px;color:#6b7280;display:block;margin-top:2px">50 رسالة + حزمة كاملة + Gemini - اشتراك</small></div>
  </div>
</div>

<div class="sidebar" id="sidebar">
  <button class="new-chat-btn" onclick="newChat()">+ محادثة جديدة</button>
  <h3>المحادثات</h3>
  <div id="chatList"></div>
</div>

<div id="chatContainer" class="chat-container">
  <div id="welcomeBox" class="welcome-box">
    <h1>Gemalot</h1>
    <div class="line"></div>
    <p>كيف يمكنني مساعدتك؟</p>
  </div>
</div>

<div class="suggestions">
  <button class="sug-btn" onclick="sendSuggestion('نواقض الوضوء')">نواقض الوضوء</button>
  <button class="sug-btn" onclick="sendSuggestion('أحكام الصيام')">أحكام الصيام</button>
  <button class="sug-btn" onclick="sendSuggestion('أركان الصلاة')">أركان الصلاة</button>
  <button class="sug-btn" onclick="sendSuggestion('الزكاة')">الزكاة</button>
</div>

<div class="input-area"><div class="input-wrapper"><textarea id="userInput" placeholder="اكتب سؤالك ..." rows="1"></textarea><button class="send-btn" id="sendBtn">إرسال</button></div></div>
<div class="full-glow" id="fullGlow"></div>
<div id="msgCounter"></div>

<script>
const MODEL_LIMITS={normal:200,pro:15,pro_plus:25,pro_gold:35,pro_extra:50};
const SOURCE_COUNTS={normal:35,pro:35,pro_plus:45,pro_gold:65,pro_extra:100};
let currentModel = localStorage.getItem("gemalot_current_model") || "normal";
const SOURCES = [
  {id:1, name:"ويكيبيديا عربي", type:"api", url: q => `https://ar.wikipedia.org/api/rest_v1/page/summary/${encodeURIComponent(q)}`, parse: d => d.extract},
  {id:2, name:"ويكيبيديا إنجليزي", type:"api", url: q => `https://en.wikipedia.org/api/rest_v1/page/summary/${encodeURIComponent(q)}`, parse: d => d.extract},
  {id:3, name:"Quran", type:"api", url: q => `https://api.alquran.cloud/v1/search/${encodeURIComponent(q)}/all/ar`, parse: d => d.data && d.data.matches && d.data.matches[0] && d.data.matches[0].text},
  {id:4, name:"Hadith", type:"api", url: q => `https://api.hadith.gading.dev/books/bukhari?search=${encodeURIComponent(q)}`, parse: d => d.data && d.data.hadiths && d.data.hadiths[0] && d.data.hadiths[0].arab},
  {id:5, name:"نواقض الوضوء", type:"local", data:{"الوضوء":"نواقض الوضوء: الخارج من السبيلين، النوم العميق، زوال العقل، مس الفرج بشهوة، أكل لحم الإبل"}},
  {id:6, name:"الصلاة", type:"local", data:{"الصلاة":"أركان الصلاة: النية، تكبيرة الإحرام، القيام، الفاتحة، الركوع، السجود، التشهد، السلام"}},
  {id:7, name:"الصيام", type:"local", data:{"الصيام":"الصيام ركن، يبطل بالأكل والشرب والجماع عمدا","رمضان":"فرض في السنة الثانية"}},
  {id:8, name:"الزكاة", type:"local", data:{"الزكاة":"تجب إذا بلغ النصاب وحال الحول، ربع العشر 2.5%"}},
  {id:9, name:"الحج", type:"local", data:{"الحج":"فرض مرة، أركانه الإحرام والوقوف بعرفة والطواف والسعي"}},
  {id:10, name:"الغسل", type:"local", data:{"الغسل":"يجب بالجنابة والحيض، يعمم الجسد بالماء"}},
  {id:11, name:"التيمم", type:"local", data:{"التيمم":"يشرع عند فقد الماء، ضربة للوجه والكفين"}},
  {id:12, name:"سجود السهو", type:"local", data:{"سجود السهو":"سجدتان لجبر النقص أو الزيادة","ما أحكام سجود السهو":"إذا شك يبني على اليقين ويسجد قبل السلام"}},
  {id:13, name:"الربا", type:"local", data:{"الربا":"محرم قطعا"}},
  {id:14, name:"النكاح", type:"local", data:{"النكاح":"سنة، أركانه الولي والشهود والمهر"}},
  {id:15, name:"التوبة", type:"local", data:{"التوبة":"الإقلاع والندم والعزم ورد الحقوق"}},
  {id:16, name:"القرآن", type:"local", data:{"القرآن":"كلام الله، 114 سورة"}},
  {id:17, name:"السيرة", type:"local", data:{"السيرة":"ولد عام الفيل، بعث 40، هاجر 53"}},
  {id:18, name:"العقيدة", type:"local", data:{"التوحيد":"ألوهية وربوبية وأسماء وصفات"}},
  {id:19, name:"الأذكار", type:"local", data:{"أذكار الصباح":"آية الكرسي والإخلاص والمعوذتين"}},
  {id:20, name:"الآداب", type:"local", data:{"آداب الطعام":"بسم الله، باليمين"}},
  {id:21, name:"المواريث", type:"local", data:{"المواريث":"العلم بمن يرث"}},
  {id:22, name:"الجنائز", type:"local", data:{"الجنازة":"غسل وتكفين وصلاة ودفن"}},
  {id:23, name:"الأطعمة", type:"local", data:{"الحلال":"كل طيب"}},
  {id:24, name:"اللباس", type:"local", data:{"لباس المرأة":"ستر البدن"}},
  {id:25, name:"الدعاء", type:"local", data:{"الدعاء":"مخ العبادة"}},
  {id:26, name:"الصحابة", type:"local", data:{"الصحابة":"أفضلهم أبو بكر ثم عمر"}},
  {id:27, name:"التفسير", type:"local", data:{"التفسير":"بيان معاني القرآن"}},
  {id:28, name:"الحديث", type:"local", data:{"الحديث":"صحيح وحسن وضعيف"}},
  {id:29, name:"الفقه", type:"local", data:{"المذاهب الأربعة":"الحنفي والمالكي والشافعي والحنبلي"}},
  {id:30, name:"أصول الفقه", type:"local", data:{"أصول الفقه":"الكتاب والسنة والإجماع والقياس"}},
  {id:31, name:"القواعد", type:"local", data:{"القواعد":"الأمور بمقاصدها، اليقين لا يزول بالشك"}},
  {id:32, name:"الشمائل", type:"local", data:{"شمائل النبي":"كان خلقه القرآن"}},
  {id:33, name:"الفتاوى", type:"local", data:{"البنوك الإسلامية":"المرابحة"}},
  {id:34, name:"الطب النبوي", type:"local", data:{"الطب النبوي":"الحجامة والعسل"}},
  {id:35, name:"الاقتصاد", type:"local", data:{"الاقتصاد":"لا ربا ولا غرر"}},
  {id:36, name:"التربية", type:"local", data:{"تربية الأبناء":"بالقدوة والرفق"}},
  {id:37, name:"النحو", type:"local", data:{"النحو":"الكلمة اسم وفعل وحرف"}},
  {id:38, name:"البلاغة", type:"local", data:{"البلاغة":"بيان ومعاني وبديع"}},
  {id:39, name:"التاريخ", type:"local", data:{"الخلافة":"أبو بكر 2س، عمر 10س"}},
  {id:40, name:"الأخلاق", type:"local", data:{"الأخلاق":"الصدق والأمانة"}},
  {id:41, name:"الأسرة", type:"local", data:{"الأسرة":"لبنة المجتمع"}},
  {id:42, name:"المرأة", type:"local", data:{"المرأة":"مكرمة"}},
  {id:43, name:"الإعجاز", type:"local", data:{"الإعجاز":"مراحل الجنين"}},
  {id:44, name:"قصص الأنبياء", type:"local", data:{"قصص الأنبياء":"آدم ونوح وإبراهيم وموسى"}},
  {id:45, name:"الأدعية", type:"local", data:{"أدعية":"اللهم إني أسألك علما نافعا"}},
  {id:46, name:"الفلسفة", type:"local", data:{"الفلسفة":"ابن سينا والفارابي"}},
  {id:47, name:"المقاصد", type:"local", data:{"المقاصد":"حفظ الدين والنفس والعقل والنسل والمال"}},
  {id:48, name:"القضاء", type:"local", data:{"القضاء":"البينة على المدعي"}},
  {id:49, name:"الحدود", type:"local", data:{"الحدود":"الزنا والسرقة والقذف"}},
  {id:50, name:"الأوقاف", type:"local", data:{"الوقف":"تحبيس الأصل"}},
  {id:51, name:"الإجارة", type:"local", data:{"الإجارة":"بيع منفعة"}},
  {id:52, name:"الأضحية", type:"local", data:{"الأضحية":"سنة مؤكدة"}},
  {id:53, name:"العقيقة", type:"local", data:{"العقيقة":"سنة للمولود"}},
  {id:54, name:"الرؤى", type:"local", data:{"الرؤى":"الرؤيا الصالحة من الله"}},
  {id:55, name:"اليوم الآخر", type:"local", data:{"اليوم الآخر":"البعث والحشر والحساب"}},
  {id:56, name:"القدر", type:"local", data:{"القدر":"الإيمان بأن الله كتب كل شيء"}},
  {id:57, name:"البرزخ", type:"local", data:{"البرزخ":"حياة القبر"}},
  {id:58, name:"الأسماء", type:"local", data:{"التسمية":"أحب الأسماء عبد الله"}},
  {id:59, name:"الذبائح", type:"local", data:{"الذبائح":"يشترط التسمية"}},
  {id:60, name:"الوكالة", type:"local", data:{"الوكالة":"نيابة"}},
  {id:61, name:"الكفالة", type:"local", data:{"الكفالة":"ضم ذمة"}},
  {id:62, name:"الرهن", type:"local", data:{"الرهن":"توثيق دين"}},
  {id:63, name:"اللقيط", type:"local", data:{"اللقيط":"طفل منبوذ"}},
  {id:64, name:"الأيمان", type:"local", data:{"الأيمان":"الحلف بالله"}},
  {id:65, name:"الهبة", type:"local", data:{"الهبة":"تمليك بلا عوض"}},
  {id:66, name:"السياسة", type:"local", data:{"السياسة":"الشورى والعدل"}},
  {id:67, name:"التصوف", type:"local", data:{"التصوف":"تزكية النفس"}},
  {id:68, name:"المنطق", type:"local", data:{"المنطق":"آلة تعصم الفكر"}},
  {id:69, name:"العروض", type:"local", data:{"العروض":"علم موازين الشعر"}},
  {id:70, name:"الرقائق", type:"local", data:{"الرقائق":"ترقيق القلوب"}},
  {id:71, name:"الجهاد", type:"local", data:{"الجهاد":"بذل الجهد"}},
  {id:72, name:"الشهادات", type:"local", data:{"الشهادة":"شروطها العدالة"}},
  {id:73, name:"التعزير", type:"local", data:{"التعزير":"عقوبة غير مقدرة"}},
  {id:74, name:"الصحابيات", type:"local", data:{"الصحابيات":"خديجة وعائشة"}},
  {id:75, name:"الغزوات", type:"local", data:{"الغزوات":"بدر وأحد والخندق"}},
  {id:76, name:"أمهات المؤمنين", type:"local", data:{"أمهات المؤمنين":"خديجة وعائشة وحفصة"}},
  {id:77, name:"التجويد", type:"local", data:{"التجويد":"إعطاء كل حرف حقه"}},
  {id:78, name:"المواريث تفصيل", type:"local", data:{"أصحاب الفروض":"الزوجة والبنت"}},
  {id:79, name:"الاقتصاد2", type:"local", data:{"الشركات":"المضاربة"}},
  {id:80, name:"التربية2", type:"local", data:{"بر الوالدين":"فرض"}},
  {id:81, name:"الشفعة", type:"local", data:{"الشفعة":"حق الشريك"}},
  {id:82, name:"اللقطة", type:"local", data:{"اللقطة":"ما يوجد ضائعا"}},
  {id:83, name:"الجعالة", type:"local", data:{"الجعالة":"جعل على عمل"}},
  {id:84, name:"السبق", type:"local", data:{"السبق":"المسابقة"}},
  {id:85, name:"النذور", type:"local", data:{"النذور":"نذر الطاعة يجب الوفاء به"}},
  {id:86, name:"الصيد", type:"local", data:{"الصيد":"بكلب معلم"}},
  {id:87, name:"الختان", type:"local", data:{"الختان":"سنة للرجال"}},
  {id:88, name:"الحوالة", type:"local", data:{"الحوالة":"نقل الدين"}},
  {id:89, name:"المساقاة", type:"local", data:{"المساقاة":"دفع الشجر"}},
  {id:90, name:"الوصايا", type:"local", data:{"الوصية":"لا وصية لوارث"}},
  {id:91, name:"الهجرة", type:"local", data:{"الهجرة":"من مكة للمدينة 622م"}},
  {id:92, name:"المؤاخاة", type:"local", data:{"المؤاخاة":"آخى بين المهاجرين والأنصار"}},
  {id:93, name:"الآداب2", type:"local", data:{"آداب السلام":"إفشاء السلام سنة"}},
  {id:94, name:"الدعاء2", type:"local", data:{"أوقات الإجابة":"الثلث الأخير"}},
  {id:95, name:"العقيدة2", type:"local", data:{"أركان الإيمان":"الله وملائكته وكتبه ورسله"}},
  {id:96, name:"السيرة2", type:"local", data:{"غزوة بدر":"أول غزوة"}},
  {id:97, name:"القرآن2", type:"local", data:{"مكي ومدني":"المكي قبل الهجرة"}},
  {id:98, name:"الصلاة2", type:"local", data:{"شروط الصلاة":"الإسلام والعقل ودخول الوقت"}},
  {id:99, name:"الصيام2", type:"local", data:{"المفطرات":"الأكل والشرب والجماع"}},
  {id:100, name:"الزكاة2", type:"local", data:{"نصاب الذهب":"85غ"}}
];

function getTodayStr(){return new Date().toISOString().split('T')[0];}
function checkAndResetDaily(){const today=getTodayStr();const last=localStorage.getItem("gemalot_daily_date");if(last!==today){localStorage.setItem("gemalot_daily_date",today);localStorage.setItem("gemalot_daily_count","0");localStorage.removeItem("gemalot_block_until");}}
function getCount(){return parseInt(localStorage.getItem("gemalot_daily_count")||"0");}
function incCount(){localStorage.setItem("gemalot_daily_count",(getCount()+1).toString());}
function getModelLimit(){return MODEL_LIMITS[currentModel]||200;}
function getSourceCount(){return SOURCE_COUNTS[currentModel]||35;}
function canSend(){const active=localStorage.getItem("gemalot_subscription_active")==="true";if(active)return true;const blockUntil=localStorage.getItem("gemalot_block_until");if(blockUntil&&Date.now()<parseInt(blockUntil))return false;return getCount()<getModelLimit();}
function updateCounterUI(){
  const counter=document.getElementById("msgCounter");
  if(!counter)return;
  const count=getCount();
  const lim=getModelLimit();
  const active=localStorage.getItem("gemalot_subscription_active")==="true";
  counter.textContent=active?`✓ مشترك مميز`:`${currentModel} - ${count}/${lim}`;
}
function toggleModelDropdown(){var d=document.getElementById("modelDropdown");if(!d)return;d.classList.toggle("open");d.style.display=d.classList.contains("open")?"block":"none";}
function selectModelFromDropdown(model){
  if(model&&model.startsWith("pro_")){goToSubscription(model);return;}
  currentModel=model;
  localStorage.setItem("gemalot_current_model",model);
  var names={normal:"Gemalot Normal ▼",pro:"Gemalot Pro ▼"};
  var b=document.getElementById("currentModelBtn");
  if(b)b.textContent=names[model]||model;
  var dd=document.getElementById("modelDropdown");
  if(dd){dd.classList.remove("open");dd.style.display="none";}
  document.querySelectorAll(".model-opt").forEach(o=>{
    var is=o.dataset.model===model;
    o.classList.toggle("active",is);
    var chk=o.querySelector(".check");
    if(chk)chk.style.display=is?"":"none";
  });
  updateCounterUI();
}
function goToSubscription(plan){localStorage.setItem("gemalot_requested_plan",plan);location.href="subscription.html?plan="+plan;}

const chatContainer=document.getElementById('chatContainer');
const userInput=document.getElementById('userInput');
const sendBtn=document.getElementById('sendBtn');
const fullGlow=document.getElementById('fullGlow');
const chatList=document.getElementById('chatList');
let currentChatId=null,messages=[];

function triggerFullGlow(){
  fullGlow.classList.remove('active');
  void fullGlow.offsetWidth;
  fullGlow.classList.add('active');
}
function getChats(){return JSON.parse(localStorage.getItem("gemalot_chats")||"[]");}
function saveChats(c){localStorage.setItem("gemalot_chats",JSON.stringify(c));}
function renderChatList(){
  chatList.innerHTML="";
  getChats().forEach(c=>{
    const div=document.createElement("div");
    div.className="chat-item";
    div.innerHTML=`<span class="chat-title">${c.title||"محادثة"}</span><button class="delete-btn">🗑️</button>`;
    div.querySelector('.chat-title').onclick=()=>loadChat(c.id);
    div.querySelector('.delete-btn').onclick=e=>{e.stopPropagation();deleteChat(c.id);};
    chatList.appendChild(div);
  });
}
function deleteChat(id){saveChats(getChats().filter(c=>c.id!==id));if(currentChatId===id)newChat();renderChatList();}
function newChat(){currentChatId=Date.now().toString();messages=[];chatContainer.innerHTML=`<div id="welcomeBox" class="welcome-box"><h1>Gemalot</h1><div class="line"></div><p>كيف يمكنني مساعدتك؟</p></div>`;document.getElementById("sidebar").classList.remove("open");updateCounterUI();}
function loadChat(id){const chat=getChats().find(c=>c.id===id);if(!chat)return;currentChatId=id;messages=chat.messages||[];chatContainer.innerHTML="";messages.forEach(m=>appendMessage(m.text,m.sender,false));document.getElementById("sidebar").classList.remove("open");}
function saveCurrentChat(){if(!messages.length)return;const chats=getChats();const title=messages[0]?.text?.slice(0,30)||"محادثة";const data={id:currentChatId||Date.now().toString(),title,messages,updated:Date.now()};const i=chats.findIndex(c=>c.id===currentChatId);if(i>-1)chats[i]=data;else chats.unshift(data);saveChats(chats);renderChatList();}
function toggleSidebar(){document.getElementById("sidebar").classList.toggle("open");}
function appendMessage(text,sender,save=true){
  const w=document.getElementById("welcomeBox");
  if(w)w.style.display="none";
  const msgDiv=document.createElement("div");
  msgDiv.className=`message ${sender}`;
  chatContainer.appendChild(msgDiv);
  window.scrollTo({top:document.body.scrollHeight,behavior:"smooth"});
  if(sender==="ai"){
    let i=0;
    (function type(){
      if(i<text.length){
        msgDiv.textContent+=text.charAt(i++);
        window.scrollTo({top:document.body.scrollHeight,behavior:"smooth"});
        setTimeout(type,10);
      }
    })();
  } else {
    msgDiv.textContent=text;
  }
  if(save){messages.push({text,sender});if(!currentChatId)currentChatId=Date.now().toString();saveCurrentChat();}
}
async function searchInSource(source, query){
  try{
    if(source.type==="local"){
      const lower=query.toLowerCase();
      for(const key in source.data){
        if(lower.includes(key.toLowerCase()) || key.toLowerCase().includes(lower)){
          return source.data[key];
        }
      }
      return null;
    } else if(source.type==="api"){
      const controller = new AbortController();
      const timeout = setTimeout(() => controller.abort(), 2000);
      const res = await fetch(source.url(query), {signal: controller.signal});
      clearTimeout(timeout);
      if(!res.ok) return null;
      const data = await res.json();
      const parsed = source.parse(data);
      if(parsed && parsed.length>10) return parsed.slice(0,400);
      return null;
    }
  }catch(e){return null;}
  return null;
}
async function searchAllSources(query, model){
  const count = SOURCE_COUNTS[model] || 35;
  const allowed = SOURCES.slice(0, count);
  for(const src of allowed){
    if(src.type==="local"){
      const r = await searchInSource(src, query);
      if(r) return r;
    }
  }
  for(const src of allowed){
    if(src.type==="api"){
      const r = await searchInSource(src, query);
      if(r) return r;
    }
  }
  return null;
}
async function processMessage(text){
  if(!text)text=userInput.value.trim();
  if(!text)return;
  checkAndResetDaily();
  if(!canSend()){
    appendMessage(`⚠️ انتهى حد ${currentModel} اليوم (${getModelLimit()} رسالة)`,"ai",true);
    return;
  }
  triggerFullGlow();
  appendMessage(text,"user");
  incCount();updateCounterUI();
  userInput.value="";
  const result = await searchAllSources(text, currentModel);
  if(result){
    appendMessage(result,"ai",true);
  } else {
    appendMessage("تعذر تحميل الإجابة - جرب: نواقض الوضوء، أحكام الصيام، أركان الصلاة، الزكاة، سجود السهو","ai",true);
  }
}
function sendSuggestion(t){processMessage(t);}
document.addEventListener('click',(e)=>{
  if(!e.target.closest('.model-dropdown')&&!e.target.closest('.model-current')){
    var d=document.getElementById("modelDropdown");
    if(d){d.classList.remove("open");d.style.display="none";}
  }
  if(!e.target.closest('.sidebar')&&!e.target.closest('.menu-btn'))document.getElementById("sidebar").classList.remove("open");
});
userInput.addEventListener("focus",triggerFullGlow);
sendBtn.addEventListener("click",()=>processMessage());
userInput.addEventListener("keydown",e=>{if(e.key==="Enter"&&!e.shiftKey){e.preventDefault();processMessage();}});
renderChatList();updateCounterUI();checkAndResetDaily();selectModelFromDropdown(currentModel);
</script>
</body>
</html>
