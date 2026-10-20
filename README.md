<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width,initial-scale=1,viewport-fit=cover">
<meta name="theme-color" content="#06152b">
<title>Eisen Calculator</title>
<link rel="icon" href="icon.png" type="image/png">
<style>
:root{--bg:#040b16;--panel:#09182d;--panel2:#0d203b;--line:#1b3555;--text:#f3f7ff;--muted:#8ea5c4;--blue:#2675f5;--cyan:#42d9ed;--danger:#ff647c;--green:#5ee0b0}
*{box-sizing:border-box}html,body{margin:0;min-height:100%;background:var(--bg);color:var(--text);font-family:system-ui,-apple-system,"Segoe UI",Tahoma,Arial,sans-serif}
body{min-height:100dvh;background:radial-gradient(ellipse at 50% -20%,#12325c 0%,#071426 42%,#040b16 78%);display:flex;justify-content:center}
button{font:inherit;color:inherit;border:0;cursor:pointer;-webkit-tap-highlight-color:transparent;touch-action:manipulation}
.app{width:min(100%,520px);min-height:100dvh;padding:18px 16px calc(22px + env(safe-area-inset-bottom));display:flex;flex-direction:column;gap:15px}
.top{display:flex;align-items:center;justify-content:space-between;gap:12px;padding:4px 2px}
.brand{display:flex;align-items:center;gap:11px;min-width:0}.brand-icon{width:45px;height:45px;border-radius:14px;background:linear-gradient(145deg,#173b69,#0b1b32);border:1px solid #2b4b72;display:grid;place-items:center;overflow:hidden;box-shadow:0 7px 22px #0005}
.brand-icon img{width:100%;height:100%;object-fit:cover}.fallback-icon{font-size:23px;color:var(--cyan);font-weight:800}
.brand h1{font-size:17px;margin:0;font-weight:800;letter-spacing:.1px}.brand p{margin:3px 0 0;color:var(--muted);font-size:11px}
.icon-btn{width:42px;height:42px;border-radius:14px;background:#0c1c31;border:1px solid var(--line);display:grid;place-items:center;font-size:19px;color:#cfe1fb}
.display{border:1px solid #203a5d;background:linear-gradient(145deg,#0b1c32,#071322);border-radius:25px;padding:20px 19px 17px;box-shadow:0 16px 40px #0003;min-height:170px;display:flex;flex-direction:column;justify-content:flex-end;gap:12px;position:relative;overflow:hidden}
.display:before{content:"";position:absolute;width:180px;height:180px;top:-125px;left:-40px;background:#2778ff18;border-radius:50%;filter:blur(2px);pointer-events:none}
.expression{font-size:18px;min-height:28px;color:#a8bdd8;direction:ltr;text-align:right;overflow-wrap:anywhere;word-break:break-word}
.result{font-size:clamp(35px,9vw,49px);font-weight:750;line-height:1.15;direction:ltr;text-align:right;overflow-wrap:anywhere;word-break:break-word;letter-spacing:-1px}
.display-bottom{display:flex;align-items:center;justify-content:space-between;color:var(--muted);font-size:11px;margin-top:3px}
.keypad{display:grid;grid-template-columns:repeat(4,minmax(0,1fr));gap:10px;flex:1}
.key{min-height:62px;border-radius:19px;background:linear-gradient(145deg,#10233d,#0b1a2d);border:1px solid #1c3451;font-size:23px;font-weight:600;box-shadow:0 5px 12px #0002;transition:transform .08s,border-color .15s,background .15s;user-select:none}
.key:active{transform:scale(.96);background:#18375c}
.key.utility{font-size:17px;color:#b8d1f1;background:#10233a}
.key.operator{background:linear-gradient(145deg,#12396c,#10284b);border-color:#23538a;color:#83c5ff}
.key.clear{color:#ff9aa9;background:#351a2b;border-color:#633044}
.key.equals{background:linear-gradient(145deg,#3187ff,#1557d5);border-color:#438df5;color:white;box-shadow:0 8px 20px #1768e955}
.key.zero{grid-column:span 2}
.bottom-row{display:flex;gap:10px}.bottom-row button{flex:1;min-height:43px;border:1px solid var(--line);border-radius:14px;background:#09182a;color:#a8bdd8;font-size:13px}
.history-panel{display:none;border:1px solid var(--line);border-radius:20px;background:#081629;padding:15px;max-height:260px;overflow:auto}
.history-panel.open{display:block}.history-head{display:flex;justify-content:space-between;align-items:center;margin-bottom:10px}.history-head h2{font-size:15px;margin:0}.history-head button{background:transparent;color:var(--danger);font-size:12px}
.history-item{width:100%;display:block;text-align:right;background:#0c1d33;border:1px solid #182f4b;border-radius:13px;padding:11px 12px;margin-top:8px}.history-expression{direction:ltr;text-align:right;color:var(--muted);font-size:12px;overflow-wrap:anywhere}.history-result{direction:ltr;text-align:right;font-size:19px;font-weight:700;margin-top:4px;overflow-wrap:anywhere}.empty{color:var(--muted);font-size:13px;text-align:center;padding:20px 0}
.toast{position:fixed;bottom:calc(22px + env(safe-area-inset-bottom));left:50%;transform:translate(-50%,15px);opacity:0;pointer-events:none;background:#172d48;border:1px solid #355575;color:white;border-radius:13px;padding:10px 15px;font-size:13px;transition:.2s;z-index:5;white-space:nowrap}.toast.show{opacity:1;transform:translate(-50%,0)}
.splash{position:fixed;inset:0;z-index:10;background:radial-gradient(circle at center,#102c50,#040b16 68%);display:grid;place-items:center;transition:opacity .5s,visibility .5s}.splash.hide{opacity:0;visibility:hidden;pointer-events:none}.splash-content{display:flex;flex-direction:column;align-items:center;gap:17px}.splash-logo{width:92px;height:92px;border-radius:27px;border:1px solid #3a6390;background:#102b4b;box-shadow:0 0 40px #2688ff3b;display:grid;place-items:center;overflow:hidden;animation:flip .9s cubic-bezier(.2,.8,.2,1) both}.splash-logo img{width:100%;height:100%;object-fit:cover}.splash-fallback{font-size:42px;font-weight:900;color:var(--cyan)}.spinner{width:37px;height:7px;border:2px solid #244c77;border-top-color:var(--cyan);border-radius:10px;animation:spin .8s linear infinite}.splash-title{font-size:18px;font-weight:800;letter-spacing:.3px}.splash-sub{font-size:11px;color:var(--muted);margin-top:-12px}
@keyframes flip{0%{transform:rotateY(0) scale(.55);opacity:0}65%{transform:rotateY(390deg) scale(1.05);opacity:1}100%{transform:rotateY(360deg) scale(1)}}
@keyframes spin{to{transform:rotate(360deg)}}
@media(min-height:850px){.app{padding-top:25px;gap:18px}.key{min-height:70px}.display{min-height:190px}}
@media(max-width:360px){.app{padding-inline:11px;gap:11px}.keypad{gap:7px}.key{min-height:55px;border-radius:15px;font-size:21px}.display{padding:16px}.result{font-size:35px}}
@media(prefers-reduced-motion:reduce){*,*:before,*:after{animation-duration:.01ms!important;transition-duration:.01ms!important}}
</style>
</head>
<body>
<div class="splash" id="splash" aria-label="Eisen Calculator">
  <div class="splash-content">
    <div class="splash-logo" id="splashLogo"><span class="splash-fallback">∑</span></div>
    <div class="spinner"></div>
    <div class="splash-title">Eisen Calculator</div>
    <div class="splash-sub">آلة حاسبة ذكية وبسيطة</div>
  </div>
</div>
<main class="app">
  <header class="top">
    <div class="brand">
      <div class="brand-icon" id="brandIcon"><span class="fallback-icon">∑</span></div>
      <div><h1>Eisen Calculator</h1><p>آلة حاسبة احترافية</p></div>
    </div>
    <button class="icon-btn" id="historyToggle" aria-label="سجل العمليات" title="سجل العمليات">◷</button>
  </header>

  <section class="display" aria-live="polite">
    <div class="expression" id="expression">0</div>
    <div class="result" id="result">0</div>
    <div class="display-bottom"><span id="status">جاهزة للحساب</span><span>Eisen</span></div>
  </section>

  <section class="history-panel" id="historyPanel">
    <div class="history-head"><h2>سجل العمليات</h2><button id="clearHistory">مسح السجل</button></div>
    <div id="historyList"><div class="empty">لا توجد عمليات محفوظة بعد</div></div>
  </section>

  <section class="keypad" id="keypad" aria-label="أزرار الآلة الحاسبة">
    <button class="key clear" data-action="clear">AC</button>
    <button class="key utility" data-action="delete">⌫</button>
    <button class="key utility" data-value="(">(</button>
    <button class="key operator" data-value=")">)</button>

    <button class="key utility" data-value="%">%</button>
    <button class="key" data-value="7">7</button>
    <button class="key" data-value="8">8</button>
    <button class="key" data-value="9">9</button>

    <button class="key operator" data-value="÷">÷</button>
    <button class="key" data-value="4">4</button>
    <button class="key" data-value="5">5</button>
    <button class="key" data-value="6">6</button>

    <button class="key operator" data-value="×">×</button>
    <button class="key" data-value="1">1</button>
    <button class="key" data-value="2">2</button>
    <button class="key" data-value="3">3</button>

    <button class="key operator" data-value="−">−</button>
    <button class="key utility" data-action="sign">+/−</button>
    <button class="key zero" data-value="0">0</button>
    <button class="key" data-value=".">.</button>

    <button class="key operator" data-value="+">+</button>
    <button class="key equals" data-action="equals" style="grid-column:span 3">=</button>
  </section>
  <div class="bottom-row">
    <button id="copyResult">نسخ النتيجة</button>
    <button id="backspaceClear">حذف آخر رقم</button>
  </div>
</main>
<div class="toast" id="toast"></div>
<script>
(function(){
  'use strict';
  const $=id=>document.getElementById(id);
  const expressionEl=$('expression'), resultEl=$('result'), statusEl=$('status');
  let expression='', justEvaluated=false, lastResult=0, history=[];
  try{history=JSON.parse(localStorage.getItem('eisen_calc_history')||'[]');if(!Array.isArray(history))history=[];}catch(_){history=[];}

  // Load icon.png when available; keep a graceful fallback if the icon is absent.
  function loadIcon(containerId, fallbackClass){
    const container=$(containerId); if(!container)return;
    const img=new Image(); img.src='icon.png'; img.alt='Alislamiah';
    img.onload=()=>{container.innerHTML='';container.appendChild(img);};
  }
  loadIcon('brandIcon'); loadIcon('splashLogo');

  function showToast(message){
    const t=$('toast');t.textContent=message;t.classList.add('show');
    clearTimeout(showToast.timer);showToast.timer=setTimeout(()=>t.classList.remove('show'),1800);
  }
  function prettyNumber(n){
    if(!Number.isFinite(n))throw new Error('نتيجة غير صالحة');
    if(Math.abs(n)<1e-12)n=0;
    return Number(n.toPrecision(12)).toLocaleString('en-US',{useGrouping:false,maximumFractionDigits:10});
  }
  function displayExpression(s){return s||'0';}
  function update(){
    expressionEl.textContent=displayExpression(expression);
    if(!expression){resultEl.textContent='0';statusEl.textContent='جاهزة للحساب';return;}
    try{
      const value=calculate(expression);
      resultEl.textContent=prettyNumber(value);
      statusEl.textContent='النتيجة المباشرة';
    }catch(e){
      resultEl.textContent=justEvaluated?'0':'…';
      statusEl.textContent='أدخل عملية صحيحة';
    }
  }
  function tokenize(input){
    const s=input.replace(/×/g,'*').replace(/÷/g,'/').replace(/−/g,'-').replace(/\s+/g,'');
    const tokens=[];let i=0;
    while(i<s.length){
      const c=s[i];
      if(/[0-9.]/.test(c)){
        let start=i,dots=0;
        while(i<s.length&&/[0-9.]/.test(s[i])){if(s[i]==='.')dots++;i++;}
        if(dots>1)throw new Error('عدد غير صالح');
        const raw=s.slice(start,i);if(raw==='.')throw new Error('عدد غير صالح');
        tokens.push({type:'num',value:Number(raw)});continue;
      }
      if('+-*/()%'.includes(c)){tokens.push({type:'op',value:c});i++;continue;}
      throw new Error('رمز غير مسموح');
    }
    return tokens;
  }
  function calculate(input){
    let tokens=tokenize(input);if(!tokens.length)throw new Error('فارغ');
    // Insert implicit multiplication: 2(3), (2)3, and adjacent parentheses.
    const expanded=[];
    for(let i=0;i<tokens.length;i++){
      const cur=tokens[i],prev=expanded[expanded.length-1];
      if(prev&&((prev.type==='num'||prev.value===')'||prev.value==='%')&&(cur.value==='('||cur.type==='num')))
        expanded.push({type:'op',value:'*'});
      expanded.push(cur);
    }
    let p=0;
    function primary(){
      let t=expanded[p];
      if(!t)throw new Error('تعبير ناقص');
      if(t.value==='+'){p++;return primary();}
      if(t.value==='-'){p++;return -primary();}
      if(t.value==='('){p++;let v=addSub();if(!expanded[p]||expanded[p].value!==')')throw new Error('قوس غير مغلق');p++;return v;}
      if(t.type==='num'){p++;let v=t.value;while(expanded[p]&&expanded[p].value==='%'){v/=100;p++;}return v;}
      throw new Error('تعبير غير صالح');
    }
    function mulDiv(){
      let v=primary();
      while(expanded[p]&&['*','/'].includes(expanded[p].value)){
        const op=expanded[p++].value,b=primary();
        if(op==='/'&&b===0)throw new Error('القسمة على صفر غير ممكنة');
        v=op==='*'?v*b:v/b;
      }
      return v;
    }
    function addSub(){
      let v=mulDiv();
      while(expanded[p]&&['+','-'].includes(expanded[p].value)){
        const op=expanded[p++].value,b=mulDiv();v=op==='+'?v+b:v-b;
      }
      return v;
    }
    const value=addSub();if(p!==expanded.length)throw new Error('تعبير غير مكتمل');
    if(!Number.isFinite(value))throw new Error('نتيجة غير صالحة');
    return value;
  }
  function append(value){
    if(justEvaluated && /[0-9.(]/.test(value))expression='';
    justEvaluated=false;
    const last=expression.slice(-1);
    if(/[+−×÷]/.test(value)){
      if(!expression&&value!=='−')return;
      if(/[+−×÷]/.test(last))expression=expression.slice(0,-1);
    }
    if(value==='.' ){
      const segment=expression.split(/[+−×÷()%]/).pop();
      if(segment.includes('.'))return;
      if(!segment)expression+='0';
    }
    expression+=value;update();
  }
  function doEquals(){
    if(!expression)return;
    try{
      const value=calculate(expression),formatted=prettyNumber(value);
      history.unshift({expression,result:formatted,stamp:Date.now()});history=history.slice(0,60);
      try{localStorage.setItem('eisen_calc_history',JSON.stringify(history));}catch(_){}
      lastResult=value;expression=formatted;justEvaluated=true;
      expressionEl.textContent='= '+formatted;resultEl.textContent=formatted;statusEl.textContent='تم الحساب';renderHistory();
    }catch(e){showToast(e.message||'تعذر حساب العملية');statusEl.textContent=e.message||'تعذر الحساب';}
  }
  function clear(){expression='';justEvaluated=false;update();}
  function backspace(){if(justEvaluated){clear();return;}expression=expression.slice(0,-1);update();}
  function toggleSign(){
    if(!expression){expression='−';update();return;}
    if(justEvaluated){expression=String(-lastResult).replace('-','−');justEvaluated=false;update();return;}
    const m=expression.match(/(\d*\.?\d+)$|(\))$/);
    if(m&&m.index!==undefined){const start=m.index;expression=expression.slice(0,start)+'−('+expression.slice(start)+')';}
    else if(/[+−×÷(]$/.test(expression))expression+='−';
    else expression='−('+expression+')';
    update();
  }
  function renderHistory(){
    const list=$('historyList');
    if(!history.length){list.innerHTML='<div class="empty">لا توجد عمليات محفوظة بعد</div>';return;}
    list.innerHTML='';
    history.forEach((item,index)=>{
      const b=document.createElement('button');b.className='history-item';b.type='button';
      const ex=document.createElement('div');ex.className='history-expression';ex.textContent=item.expression;
      const res=document.createElement('div');res.className='history-result';res.textContent='= '+item.result;
      b.append(ex,res);b.addEventListener('click',()=>{expression=item.result;lastResult=Number(item.result);justEvaluated=true;update();$('historyPanel').classList.remove('open');});list.appendChild(b);
    });
  }
  $('keypad').addEventListener('click',e=>{
    const b=e.target.closest('button');if(!b)return;
    if(b.dataset.action==='clear')clear();
    else if(b.dataset.action==='delete')backspace();
    else if(b.dataset.action==='equals')doEquals();
    else if(b.dataset.action==='sign')toggleSign();
    else if(b.dataset.value!==undefined)append(b.dataset.value);
  });
  $('historyToggle').addEventListener('click',()=>{$('historyPanel').classList.toggle('open');renderHistory();});
  $('clearHistory').addEventListener('click',()=>{history=[];try{localStorage.removeItem('eisen_calc_history');}catch(_){}renderHistory();showToast('تم مسح سجل العمليات');});
  $('backspaceClear').addEventListener('click',backspace);
  $('copyResult').addEventListener('click',async()=>{
    let text=resultEl.textContent;
    if(!text||text==='…'){showToast('لا توجد نتيجة لنسخها');return;}
    try{await navigator.clipboard.writeText(text);showToast('تم نسخ النتيجة');}
    catch(_){const ta=document.createElement('textarea');ta.value=text;document.body.appendChild(ta);ta.select();try{document.execCommand('copy');showToast('تم نسخ النتيجة');}catch(e){showToast('تعذر النسخ على هذا المتصفح');}ta.remove();}
  });
  document.addEventListener('keydown',e=>{
    if(e.ctrlKey||e.metaKey||e.altKey)return;
    if(/^[0-9.]$/.test(e.key)){append(e.key);return;}
    const map={'+':'+','-':'−','*':'×','/':'÷','(':'(',')':')','%':'%','Enter':'equals','=':'equals','Backspace':'delete','Escape':'clear'};
    if(map[e.key]){e.preventDefault();const a=map[e.key];if(a==='equals')doEquals();else if(a==='delete')backspace();else if(a==='clear')clear();else append(a);}
  });
  renderHistory();update();
  window.setTimeout(()=>$('splash').classList.add('hide'),1500);
})();
</script>
</body>
</html>