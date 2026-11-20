# custom-document
Электронный реестр электронных документов
<!DOCTYPE html>
<html lang="ru">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>Электронный реестр таможенных документов</title>
<script>
try{ if(localStorage.getItem('er_theme')==='dark') document.documentElement.setAttribute('data-theme','dark'); }catch(e){}
</script>
<style>
*{box-sizing:border-box}
[hidden]{display:none!important}
:root{
  --bg:#f3f4f6; --card:#fff; --text:#2b2f36; --muted:#6b7280;
  --border:#e3e6ea; --accent:#4d6b8a; --accent-hover:#3f5a75;
  --accent-soft:#eef2f7; --danger:#a04a4a; --ok:#4a7c59;
  --row-hover:#fafbfc;
}
[data-theme="dark"]{
  --bg:#1a1c20; --card:#25282d; --text:#e4e6ea; --muted:#8f97a3;
  --border:#383d44; --accent:#7a9cbd; --accent-hover:#92b3d4;
  --accent-soft:#2f3640; --danger:#c98888; --ok:#86b193;
  --row-hover:#2c3036;
}
html,body{margin:0;padding:0}
body{font-family:system-ui,-apple-system,"Segoe UI",Roboto,Arial,sans-serif;
  background:var(--bg);color:var(--text);font-size:15px;line-height:1.45;
  transition:background-color .2s,color .2s}
h1,h2,h3{margin:0 0 10px;font-weight:600}
h1{font-size:17px}h2{font-size:16px}
.small{font-size:13px}.muted{color:var(--muted)}

/* ---------- auth ---------- */
.auth-wrap{min-height:100vh;display:flex;align-items:center;justify-content:center;padding:20px}
.auth-card{background:var(--card);border:1px solid var(--border);border-radius:14px;
  padding:26px;width:100%;max-width:400px;box-shadow:0 6px 24px rgba(20,25,35,.06)}
.brand{display:flex;gap:12px;align-items:center;margin-bottom:20px}
.brand-mark{width:42px;height:42px;flex:none;border-radius:10px;background:var(--accent);
  color:#fff;display:flex;align-items:center;justify-content:center;font-weight:700;font-size:15px}
.brand h1{line-height:1.3;margin:0}
.segmented{display:flex;background:var(--accent-soft);border-radius:9px;padding:4px;margin-bottom:18px;gap:4px}
.seg{flex:1;border:0;background:transparent;padding:8px;border-radius:7px;cursor:pointer;
  font:inherit;color:var(--muted)}
.seg.active{background:var(--card);color:var(--text);font-weight:600;box-shadow:0 1px 3px rgba(0,0,0,.08)}

/* ---------- forms ---------- */
.form{display:flex;flex-direction:column;gap:14px}
label{display:flex;flex-direction:column;gap:6px;font-size:13px;color:var(--muted);font-weight:500}
input,select,textarea{font:inherit;color:var(--text);background:var(--card);border:1px solid var(--border);
  border-radius:8px;padding:9px 11px;width:100%}
input:focus,select:focus,textarea:focus{outline:none;border-color:var(--accent);
  box-shadow:0 0 0 3px rgba(77,107,138,.18)}
textarea{resize:vertical}
.hint{font-size:12.5px;color:var(--muted);margin:0}
a{color:var(--accent);text-decoration:none}
a:hover{text-decoration:underline}

/* ---------- toggle switch ---------- */
.toggle-row{display:flex;justify-content:space-between;align-items:center;gap:14px;
  cursor:pointer;font-size:14px;padding:4px 0;color:var(--text);flex-direction:row}
.toggle-row input{display:none}
.toggle-switch{width:44px;height:24px;background:var(--border);border-radius:99px;
  position:relative;transition:.2s;flex:none}
.toggle-switch::after{content:'';position:absolute;left:3px;top:3px;width:18px;height:18px;
  background:#fff;border-radius:50%;transition:.2s;box-shadow:0 1px 3px rgba(0,0,0,.15)}
.toggle-row input:checked + .toggle-switch{background:var(--accent)}
.toggle-row input:checked + .toggle-switch::after{transform:translateX(20px)}

/* ---------- buttons ---------- */
.btn{font:inherit;border:1px solid var(--border);background:var(--card);color:var(--text);
  padding:9px 14px;border-radius:8px;cursor:pointer;font-weight:500;transition:.15s}
.btn:hover{background:var(--row-hover)}
.btn.primary{background:var(--accent);border-color:var(--accent);color:#fff}
.btn.primary:hover{background:var(--accent-hover);border-color:var(--accent-hover)}
.btn.ghost{background:transparent;border-color:transparent;color:var(--muted)}
.btn.ghost:hover{background:var(--accent-soft);color:var(--text)}
.btn.small{padding:5px 9px;font-size:12.5px}
.btn.workflow{background:var(--accent-soft);border-color:transparent;color:var(--accent);font-weight:600}
.btn.workflow:hover{background:var(--accent-soft);filter:brightness(.95)}
.btn.danger{color:var(--danger);border-color:#eedcdc}
.btn.danger:hover{background:#faf1f1}
[data-theme="dark"] .btn.danger{border-color:#4a2d2d}
[data-theme="dark"] .btn.danger:hover{background:#3a2626}

/* ---------- topbar ---------- */
.topbar{background:var(--card);border-bottom:1px solid var(--border);position:sticky;top:0;z-index:20}
.topbar-inner{max-width:1120px;margin:0 auto;padding:12px 20px;display:flex;
  align-items:center;justify-content:space-between;gap:12px;flex-wrap:wrap}
.small-brand{display:flex;align-items:center;gap:10px;font-weight:600;font-size:14.5px;margin:0}
.small-brand .brand-mark{width:32px;height:32px;font-size:12.5px;border-radius:8px}
.userbox{display:flex;align-items:center;gap:10px;flex-wrap:wrap}
.role-badge{font-size:11.5px;padding:2px 8px;border-radius:99px;
  background:var(--accent-soft);color:var(--accent);font-weight:600}
.role-badge.admin{background:#f0e6f0;color:#8a4d8a}
[data-theme="dark"] .role-badge.admin{background:#3d2a3d;color:#c394c3}
.tabs{max-width:1120px;margin:0 auto;padding:0 20px;display:flex;gap:4px;overflow-x:auto}
.tab{border:0;background:transparent;font:inherit;color:var(--muted);padding:10px 14px;
  cursor:pointer;border-bottom:2px solid transparent;margin-bottom:-1px;white-space:nowrap}
.tab.active{color:var(--accent);border-bottom-color:var(--accent);font-weight:600}
.container{max-width:1120px;margin:0 auto;padding:22px 20px 60px}

/* ---------- docs ---------- */
.toolbar{display:flex;gap:10px;flex-wrap:wrap;margin-bottom:18px}
.toolbar .input{flex:1;min-width:180px}
.toolbar select{width:auto}
.doc-grid{display:grid;grid-template-columns:repeat(auto-fill,minmax(290px,1fr));gap:14px}
.doc-card{background:var(--card);border:1px solid var(--border);border-radius:12px;
  padding:16px;display:flex;flex-direction:column;gap:10px}
.doc-head{display:flex;justify-content:space-between;gap:10px;align-items:flex-start}
.doc-num{font-weight:600;font-size:14.5px;word-break:break-word}
.doc-type{font-size:12.5px;color:var(--muted)}
.doc-route{font-size:13.5px}
.doc-route .arrow{color:var(--muted);margin:0 6px}
.doc-meta{font-size:12.5px;color:var(--muted);display:flex;flex-direction:column;gap:2px}
.doc-foot{display:flex;justify-content:space-between;align-items:center;margin-top:auto;
  padding-top:10px;border-top:1px solid var(--border);gap:8px;flex-wrap:wrap}
.doc-actions{display:flex;gap:6px;flex-wrap:wrap}
.badge{font-size:11.5px;font-weight:600;padding:3px 9px;border-radius:99px;white-space:nowrap}
.badge.draft{background:#eef0f2;color:#5b6470}
.badge.check{background:#f3efdf;color:#8a7434}
.badge.approved{background:#e3eef5;color:#3a6b8a}
.badge.reg{background:#e4efe7;color:var(--ok)}
.badge.reject{background:#f4e6e6;color:var(--danger)}
.badge.arch{background:#ecebee;color:#6a6472}
[data-theme="dark"] .badge.draft{background:#383d44;color:#a8b0ba}
[data-theme="dark"] .badge.check{background:#4a3f27;color:#d4bf7c}
[data-theme="dark"] .badge.approved{background:#2a4155;color:#8db8d4}
[data-theme="dark"] .badge.reg{background:#2a4a37;color:#8fc0a1}
[data-theme="dark"] .badge.reject{background:#4a2d2d;color:#d49a9a}
[data-theme="dark"] .badge.arch{background:#35323a;color:#a8a3b0}
.empty{text-align:center;color:var(--muted);padding:40px 0}

/* ---------- admin ---------- */
.card{background:var(--card);border:1px solid var(--border);border-radius:12px;
  padding:18px;margin-bottom:16px}
.card-head{display:flex;justify-content:space-between;align-items:center;gap:12px;
  margin-bottom:14px;flex-wrap:wrap}
.card-head h2{margin:0}
.stat-grid{display:grid;grid-template-columns:repeat(auto-fit,minmax(150px,1fr));
  gap:14px;margin-bottom:16px}
.stat{background:var(--card);border:1px solid var(--border);border-radius:12px;padding:16px}
.stat .num{font-size:26px;font-weight:700;color:var(--accent)}
.stat .lbl{font-size:12.5px;color:var(--muted);margin-top:2px}
.chart{display:flex;flex-direction:column;gap:10px}
.chart-row{display:grid;grid-template-columns:130px 1fr 40px;gap:10px;
  align-items:center;font-size:13px}
.chart-bar-wrap{background:var(--accent-soft);border-radius:6px;height:16px;overflow:hidden}
.chart-bar{height:100%;background:var(--accent);border-radius:6px;transition:width .4s}
.daily-chart{display:flex;align-items:flex-end;gap:6px;height:160px;
  padding:8px 0;border-bottom:1px solid var(--border)}
.day-col{flex:1;display:flex;flex-direction:column;align-items:center;gap:4px;min-width:0}
.day-bar-wrap{width:100%;flex:1;display:flex;align-items:flex-end}
.day-bar{width:100%;background:var(--accent);border-radius:4px 4px 0 0;min-height:2px;
  transition:height .3s}
.day-bar:hover{background:var(--accent-hover)}
.day-count{font-size:11px;color:var(--muted);font-weight:600}
.day-label{font-size:10.5px;color:var(--muted);text-align:center;line-height:1.1}
.table-wrap{overflow-x:auto}
table{width:100%;border-collapse:collapse;font-size:13.5px;min-width:520px}
th,td{text-align:left;padding:9px 10px;border-bottom:1px solid var(--border);white-space:nowrap}
th{font-size:12px;text-transform:uppercase;letter-spacing:.4px;color:var(--muted);font-weight:600}
tbody tr:hover{background:var(--row-hover)}
.role-select{min-width:130px;padding:5px 8px;font-size:12.5px}

/* ---------- modal / toast ---------- */
.modal{position:fixed;inset:0;background:rgba(25,30,40,.5);display:flex;
  align-items:center;justify-content:center;padding:16px;z-index:50}
.modal-card{background:var(--card);border-radius:14px;width:100%;max-width:560px;max-height:92vh;
  overflow:auto;padding:20px;box-shadow:0 20px 50px rgba(0,0,0,.25)}
.modal-head{display:flex;justify-content:space-between;align-items:center;margin-bottom:16px}
.grid2{display:grid;grid-template-columns:1fr 1fr;gap:14px}
.grid2 .full{grid-column:1/-1}
.modal-actions{display:flex;justify-content:flex-end;gap:10px}
.toast{position:fixed;left:50%;bottom:24px;transform:translateX(-50%);background:#2b2f36;
  color:#fff;padding:11px 18px;border-radius:10px;font-size:13.5px;z-index:100;
  box-shadow:0 8px 24px rgba(0,0,0,.25);max-width:calc(100% - 40px);text-align:center}
@media(max-width:560px){
  .grid2{grid-template-columns:1fr}
  .chart-row{grid-template-columns:96px 1fr 34px;font-size:12px}
  .day-label{font-size:9px}
}
</style>
</head>
<body>

<!-- ================= AUTH ================= -->
<div id="auth" class="auth-wrap">
  <div class="auth-card">
    <div class="brand">
      <div class="brand-mark">ЭР</div>
      <div>
        <h1>Электронный реестр<br>таможенных документов</h1>
        <p class="muted small">Внутренняя система учёта</p>
      </div>
    </div>

    <div class="segmented" id="authTabs">
      <button class="seg active" data-tab="login" type="button">Вход</button>
      <button class="seg" data-tab="register" type="button">Регистрация</button>
    </div>

    <form id="loginForm" class="form">
      <label>Логин<input id="loginLogin" autocomplete="username" required></label>
      <label>Пароль<input id="loginPass" type="password" autocomplete="current-password" required></label>
      <button class="btn primary" type="submit">Войти</button>
      <p class="hint" style="text-align:center;margin-top:2px">
        <a href="#" id="forgotLink">Забыли пароль?</a>
      </p>
      <p class="hint">Первый зарегистрированный пользователь становится императором.</p>
    </form>

    <form id="registerForm" class="form" hidden>
      <label>Логин<input id="regLogin" autocomplete="username" required></label>
      <label>Пароль<input id="regPass" type="password" autocomplete="new-password" required></label>
      <label>Повторите пароль<input id="regPass2" type="password" autocomplete="new-password" required></label>
      <label>Контрольное слово (необязательно)
        <input id="regRecovery" placeholder="Например, кличка питомца">
      </label>
      <p class="hint">Нужно для восстановления пароля, если забудете. Можно не указывать — тогда сброс делает император.</p>
      <button class="btn primary" type="submit">Создать аккаунт</button>
    </form>

    <form id="forgotForm" class="form" hidden>
      <p class="muted small" style="margin:0">Введите логин и контрольное слово, указанное при регистрации.</p>
      <label>Логин<input id="forgotLogin" required></label>
      <label>Контрольное слово<input id="forgotWord" required></label>
      <label>Новый пароль<input id="forgotPass" type="password" required minlength="4"></label>
      <label>Повторите пароль<input id="forgotPass2" type="password" required></label>
      <button class="btn primary" type="submit">Сбросить пароль</button>
      <p class="hint" style="text-align:center"><a href="#" id="forgotBack">← Вернуться ко входу</a></p>
      <p class="hint">Если контрольное слово не задано — обратитесь к императору.</p>
    </form>
  </div>
</div>

<!-- ================= APP ================= -->
<div id="app" hidden>
  <header class="topbar">
    <div class="topbar-inner">
      <div class="small-brand">
        <div class="brand-mark">ЭР</div>
        <span>Реестр таможенных документов</span>
      </div>
      <div class="userbox">
        <span id="userName"></span>
        <span class="role-badge" id="userRole"></span>
        <button class="btn ghost" id="logoutBtn" type="button">Выйти</button>
      </div>
    </div>
    <nav class="tabs" id="tabs">
      <button class="tab active" data-view="docs" type="button">Реестр документов</button>
      <button class="tab" data-view="admin" id="adminTab" hidden type="button">Админ-панель</button>
      <button class="tab" data-view="settings" type="button">Настройки</button>
    </nav>
  </header>

  <main class="container">
    <!-- DOCS -->
    <section id="view-docs">
      <div class="toolbar">
        <input id="searchInput" class="input" placeholder="Поиск по номеру, отправителю, получателю...">
        <select id="statusFilter" class="input"></select>
        <button class="btn" id="exportDocsBtn" type="button">Экспорт CSV</button>
        <button class="btn primary" id="addDocBtn" type="button">+ Новый документ</button>
      </div>
      <div id="docsList" class="doc-grid"></div>
      <p id="docsEmpty" class="empty" hidden>Документов не найдено</p>
    </section>

    <!-- ADMIN -->
    <section id="view-admin" hidden>
      <div class="stat-grid" id="statGrid"></div>

      <div class="card">
        <h2>Входы за последние 14 дней</h2>
        <div id="dailyChart" class="daily-chart"></div>
      </div>

      <div class="card">
        <h2>Действия по типам</h2>
        <div id="actionChart" class="chart"></div>
      </div>

      <div class="card">
        <h2>Пользователи</h2>
        <div class="table-wrap"><table id="usersTable"></table></div>
      </div>

      <div class="card">
        <div class="card-head">
          <h2>Журнал действий</h2>
          <button class="btn" id="exportLogBtn" type="button">Экспорт журнала CSV</button>
        </div>
        <div class="table-wrap"><table id="logTable"></table></div>
      </div>
    </section>

    <!-- SETTINGS -->
    <section id="view-settings" hidden>
      <div class="card">
        <h2>Оформление</h2>
        <label class="toggle-row">
          <span>Тёмная тема</span>
          <input type="checkbox" id="darkThemeToggle">
          <span class="toggle-switch"></span>
        </label>
      </div>
      <div class="card">
        <h2>Смена пароля</h2>
        <form id="changePassForm" class="form" style="max-width:400px">
          <label>Текущий пароль<input id="cpCurrent" type="password" required autocomplete="current-password"></label>
          <label>Новый пароль<input id="cpNew" type="password" required minlength="4" autocomplete="new-password"></label>
          <label>Повторите новый пароль<input id="cpNew2" type="password" required autocomplete="new-password"></label>
          <button class="btn primary" type="submit">Сменить пароль</button>
        </form>
      </div>
    </section>
  </main>
</div>

<!-- ================= MODAL: документ ================= -->
<div id="modal" class="modal" hidden>
  <div class="modal-card">
    <div class="modal-head">
      <h2 id="modalTitle">Документ</h2>
      <button class="btn ghost" id="modalClose" type="button">✕</button>
    </div>
    <form id="docForm" class="form grid2">
      <label class="full">Регистрационный номер<input id="f_reg" required placeholder="ТД-2026-0001"></label>
      <label>Тип документа<select id="f_type"></select></label>
      <label>Дата документа<input id="f_date" type="date" required></label>
      <label>Отправитель<input id="f_sender" required></label>
      <label>Получатель<input id="f_recipient" required></label>
      <label class="full" id="f_statusWrap">Статус<select id="f_status"></select></label>
      <label class="full">Примечание<textarea id="f_note" rows="3"></textarea></label>
      <div class="full modal-actions">
        <button type="button" class="btn ghost" id="modalCancel">Отмена</button>
        <button type="submit" class="btn primary">Сохранить</button>
      </div>
    </form>
  </div>
</div>

<!-- ================= MODAL: сброс пароля императором ================= -->
<div id="resetModal" class="modal" hidden>
  <div class="modal-card" style="max-width:400px">
    <div class="modal-head">
      <h2>Сброс пароля</h2>
      <button class="btn ghost" id="resetClose" type="button">✕</button>
    </div>
    <p class="muted small" id="resetUserLabel" style="margin:0 0 14px"></p>
    <form id="resetForm" class="form">
      <label>Новый пароль<input id="resetPass" type="password" required minlength="4"></label>
      <label>Повторите пароль<input id="resetPass2" type="password" required></label>
      <div class="modal-actions">
        <button type="button" class="btn ghost" id="resetCancel">Отмена</button>
        <button type="submit" class="btn primary">Сохранить</button>
      </div>
    </form>
  </div>
</div>

<div id="toast" class="toast" hidden></div>

<script>
(function(){
'use strict';

/* ================= storage ================= */
const KEYS = {users:'er_users', docs:'er_docs', log:'er_log', session:'er_session'};
const load = (k,d)=>{ try{ const v=localStorage.getItem(k); return v?JSON.parse(v):d; }catch(e){ return d; } };
const save = (k,v)=>localStorage.setItem(k,JSON.stringify(v));
const uid  = ()=>Date.now().toString(36)+Math.random().toString(36).slice(2,8);
const $    = s=>document.querySelector(s);
const $$   = s=>document.querySelectorAll(s);

function hash(s){ let h=5381; for(let i=0;i<s.length;i++){ h=((h<<5)+h+s.charCodeAt(i))|0; } return (h>>>0).toString(16); }
function esc(s){ return String(s==null?'':s).replace(/[&<>"']/g,c=>({'&':'&amp;','<':'&lt;','>':'&gt;','"':'&quot;',"'":'&#39;'}[c])); }
const fmt = ts=> ts? new Date(ts).toLocaleString('ru-RU',{day:'2-digit',month:'2-digit',year:'numeric',hour:'2-digit',minute:'2-digit'}) : '—';
const fmtDate = d=> d? new Date(d).toLocaleDateString('ru-RU') : '—';

/* ================= роли и статусы ================= */
const ROLES = {
  admin:     {label:'Император'},
  operator:  {label:'Пользователь'},
  inspector: {label:'Проверяющий'},
  registrar: {label:'Регистратор'}
};
const DOC_TYPES = ['Декларация на товары','Транзитная декларация','Сертификат происхождения','Инвойс','Транспортный документ','Разрешение / лицензия','Акт таможенного досмотра','Прочее'];
const DOC_STATUSES = [
  {v:'draft',    t:'Черновик'},
  {v:'check',    t:'На проверке'},
  {v:'approved', t:'Одобрен'},
  {v:'reg',      t:'Зарегистрирован'},
  {v:'reject',   t:'Отклонён'},
  {v:'arch',     t:'В архиве'}
];
const statusLabel = v=>(DOC_STATUSES.find(s=>s.v===v)||{t:v}).t;
const roleLabel = r=>(ROLES[r]||{label:r}).label;

let currentUser = null;
let editingId = null;
let resetUserId = null;

/* ================= toast ================= */
let toastTimer;
function toast(msg){
  const t=$('#toast'); t.textContent=msg; t.hidden=false;
  clearTimeout(toastTimer);
  toastTimer=setTimeout(()=>{ t.hidden=true; },2800);
}

/* ================= журнал ================= */
function logAction(action, details){
  const log = load(KEYS.log, []);
  log.unshift({id:uid(), ts:Date.now(), login: currentUser?currentUser.login:'—',
    role: currentUser?currentUser.role:'—', action, details: details||''});
  if (log.length>2000) log.length=2000;
  save(KEYS.log, log);
}

/* ================= select заполнение ================= */
function fillSelects(){
  $('#f_type').innerHTML = DOC_TYPES.map(t=>`<option>${esc(t)}</option>`).join('');
  $('#f_status').innerHTML = DOC_STATUSES.map(s=>`<option value="${s.v}">${esc(s.t)}</option>`).join('');
  $('#statusFilter').innerHTML = '<option value="">Все статусы</option>' +
    DOC_STATUSES.map(s=>`<option value="${s.v}">${esc(s.t)}</option>`).join('');
}

/* ================= права ================= */
function roleCanCreate(role){ return role==='operator' || role==='admin'; }
function roleCanEditDoc(user, doc){
  if(!user) return false;
  if(user.role==='admin') return true;
  if(user.role==='operator' && doc.createdBy===user.login && (doc.status==='draft' || doc.status==='reject')) return true;
  return false;
}
function roleCanDeleteDoc(user, doc){ return roleCanEditDoc(user, doc); }

function getAvailableTransitions(user, doc){
  const t = [];
  const r = user.role, s = doc.status;
  if(r==='admin'){
    if(s==='draft') t.push({to:'check', label:'На проверку'});
    if(s==='check'){ t.push({to:'approved', label:'Одобрить'}); t.push({to:'reject', label:'Отклонить'}); }
    if(s==='approved') t.push({to:'reg', label:'Зарегистрировать'});
    if(s==='reject') t.push({to:'draft', label:'Вернуть в работу'});
    if(s==='reg') t.push({to:'arch', label:'В архив'});
    if(s==='arch') t.push({to:'reg', label:'Вернуть из архива'});
  } else if(r==='operator' && doc.createdBy===user.login){
    if(s==='draft') t.push({to:'check', label:'На проверку'});
    if(s==='reject') t.push({to:'draft', label:'Вернуть в работу'});
  } else if(r==='inspector'){
    if(s==='check'){ t.push({to:'approved', label:'Одобрить'}); t.push({to:'reject', label:'Отклонить'}); }
  } else if(r==='registrar'){
    if(s==='approved') t.push({to:'reg', label:'Зарегистрировать'});
    if(s==='reg') t.push({to:'arch', label:'В архив'});
    if(s==='arch') t.push({to:'reg', label:'Вернуть из архива'});
  }
  return t;
}

/* ================= тема ================= */
function applyTheme(dark){
  if(dark) document.documentElement.setAttribute('data-theme','dark');
  else document.documentElement.removeAttribute('data-theme');
  localStorage.setItem('er_theme', dark?'dark':'light');
  const cb = $('#darkThemeToggle');
  if(cb) cb.checked = dark;
}
$('#darkThemeToggle').onchange = e => {
  applyTheme(e.target.checked);
  toast(e.target.checked?'Тёмная тема включена':'Светлая тема включена');
};

/* ================= auth tabs ================= */
$$('#authTabs .seg').forEach(btn=>{
  btn.onclick = ()=>{
    $$('#authTabs .seg').forEach(b=>b.classList.toggle('active', b===btn));
    $('#forgotForm').hidden = true;
    $('#loginForm').hidden = btn.dataset.tab!=='login';
    $('#registerForm').hidden = btn.dataset.tab!=='register';
  };
});

$('#forgotLink').onclick = e => {
  e.preventDefault();
  $('#loginForm').hidden = true;
  $('#registerForm').hidden = true;
  $('#forgotForm').hidden = false;
};
$('#forgotBack').onclick = e => {
  e.preventDefault();
  $('#forgotForm').hidden = true;
  $('#loginForm').hidden = false;
};

/* ================= login ================= */
$('#loginForm').onsubmit = e=>{
  e.preventDefault();
  const login=$('#loginLogin').value.trim();
  const pass=$('#loginPass').value;
  const users=load(KEYS.users,[]);
  const u=users.find(x=>x.login.toLowerCase()===login.toLowerCase());
  if(!u || u.passHash!==hash(pass)){ toast('Неверный логин или пароль'); return; }
  u.lastLogin=Date.now(); save(KEYS.users,users);
  currentUser=u; save(KEYS.session,{id:u.id});
  logAction('Вход','Успешный вход в систему');
  $('#loginForm').reset();
  showApp();
};

/* ================= register ================= */
$('#registerForm').onsubmit = e=>{
  e.preventDefault();
  const login=$('#regLogin').value.trim();
  const pass=$('#regPass').value;
  const pass2=$('#regPass2').value;
  const recovery=$('#regRecovery').value.trim();
  if(login.length<3) return toast('Логин должен быть не короче 3 символов');
  if(!/^[a-zA-Zа-яА-Я0-9_.-]+$/.test(login)) return toast('Логин: буквы, цифры, _ . -');
  if(pass.length<4) return toast('Пароль должен быть не короче 4 символов');
  if(pass!==pass2) return toast('Пароли не совпадают');
  const users=load(KEYS.users,[]);
  if(users.some(u=>u.login.toLowerCase()===login.toLowerCase())) return toast('Такой логин уже занят');
  const isFirst = users.length===0;
  const u={
    id:uid(), login, passHash:hash(pass), role: isFirst?'admin':'operator',
    createdAt:Date.now(), lastLogin:Date.now(),
    recoveryHash: recovery ? hash(recovery) : null
  };
  users.push(u); save(KEYS.users,users);
  currentUser=u; save(KEYS.session,{id:u.id});
  logAction('Регистрация', isFirst?'Создан император системы':'Создан новый пользователь');
  $('#registerForm').reset();
  showApp();
  toast(isFirst?'Аккаунт императора создан':'Регистрация успешна');
};

/* ================= forgot password ================= */
$('#forgotForm').onsubmit = e => {
  e.preventDefault();
  const login = $('#forgotLogin').value.trim();
  const word  = $('#forgotWord').value;
  const np    = $('#forgotPass').value;
  const np2   = $('#forgotPass2').value;
  if(np.length<4) return toast('Пароль должен быть не короче 4 символов');
  if(np!==np2) return toast('Пароли не совпадают');
  const users = load(KEYS.users, []);
  const u = users.find(x=>x.login.toLowerCase()===login.toLowerCase());
  if(!u) return toast('Пользователь не найден');
  if(!u.recoveryHash) return toast('Контрольное слово не задано. Обратитесь к императору.');
  if(hash(word) !== u.recoveryHash) return toast('Неверное контрольное слово');
  u.passHash = hash(np);
  save(KEYS.users, users);
  const log = load(KEYS.log, []);
  log.unshift({id:uid(), ts:Date.now(), login:u.login, role:u.role,
    action:'Сброс пароля', details:'По контрольному слову'});
  save(KEYS.log, log);
  $('#forgotForm').reset();
  $('#forgotForm').hidden = true;
  $('#loginForm').hidden = false;
  toast('Пароль сброшен. Войдите с новым паролем.');
};

/* ================= смена пароля ================= */
$('#changePassForm').onsubmit = e => {
  e.preventDefault();
  const cur  = $('#cpCurrent').value;
  const np   = $('#cpNew').value;
  const np2  = $('#cpNew2').value;
  if(hash(cur) !== currentUser.passHash) return toast('Текущий пароль неверен');
  if(np.length<4) return toast('Новый пароль должен быть не короче 4 символов');
  if(np!==np2) return toast('Пароли не совпадают');
  const users = load(KEYS.users, []);
  const u = users.find(x=>x.id===currentUser.id);
  if(!u) return;
  u.passHash = hash(np);
  save(KEYS.users, users);
  currentUser = u;
  logAction('Смена пароля', 'Пользователь сменил пароль');
  $('#changePassForm').reset();
  toast('Пароль изменён');
};

/* ================= показ приложения ================= */
function showAuth(){ $('#auth').hidden=false; $('#app').hidden=true; }

function showApp(){
  $('#auth').hidden=true; $('#app').hidden=false;
  $('#userName').textContent=currentUser.login;
  $('#userRole').textContent=roleLabel(currentUser.role);
  $('#userRole').className='role-badge'+(currentUser.role==='admin'?' admin':'');
  $('#adminTab').hidden = currentUser.role!=='admin';
  $('#addDocBtn').hidden = !roleCanCreate(currentUser.role);
  applyTheme(document.documentElement.hasAttribute('data-theme'));
  switchView('docs');
  renderDocs();
}

$('#logoutBtn').onclick = ()=>{
  logAction('Выход','Пользователь вышел из системы');
  currentUser=null;
  localStorage.removeItem(KEYS.session);
  showAuth();
};

/* ================= views ================= */
function switchView(view){
  $$('#tabs .tab').forEach(b=>b.classList.toggle('active', b.dataset.view===view));
  $('#view-docs').hidden = view!=='docs';
  $('#view-admin').hidden = view!=='admin';
  $('#view-settings').hidden = view!=='settings';
  if(view==='admin') renderAdmin();
}
$$('#tabs .tab').forEach(b=> b.onclick=()=>switchView(b.dataset.view));

/* ================= документы ================= */
function nextRegNumber(){
  const docs=load(KEYS.docs,[]);
  const y=new Date().getFullYear();
  const n=docs.filter(d=>(d.regNumber||'').includes(String(y))).length+1;
  return `ТД-${y}-${String(n).padStart(4,'0')}`;
}

function getFilteredDocs(){
  const docs=load(KEYS.docs,[]);
  const q=$('#searchInput').value.trim().toLowerCase();
  const st=$('#statusFilter').value;
  return docs.filter(d=>{
    if(st && d.status!==st) return false;
    if(!q) return true;
    return [d.regNumber,d.sender,d.recipient,d.type,d.note,d.createdBy].join(' ').toLowerCase().includes(q);
  });
}

function renderDocs(){
  const list = getFilteredDocs();
  $('#docsList').innerHTML = list.map(d=>{
    const canEdit = roleCanEditDoc(currentUser, d);
    const canDel = roleCanDeleteDoc(currentUser, d);
    const transitions = getAvailableTransitions(currentUser, d);
    return `
    <article class="doc-card">
      <div class="doc-head">
        <div>
          <div class="doc-num">${esc(d.regNumber)}</div>
          <div class="doc-type">${esc(d.type)}</div>
        </div>
        <span class="badge ${esc(d.status)}">${esc(statusLabel(d.status))}</span>
      </div>
      <div class="doc-route">${esc(d.sender)}<span class="arrow">→</span>${esc(d.recipient)}</div>
      <div class="doc-meta">
        <span>Дата документа: ${fmtDate(d.date)}</span>
        ${d.note?`<span>${esc(d.note)}</span>`:''}
        <span>Добавил: ${esc(d.createdBy)} · ${fmt(d.createdAt)}</span>
      </div>
      ${(canEdit||canDel||transitions.length) ? `
      <div class="doc-foot">
        <div class="doc-actions">
          ${canEdit?`<button class="btn small" data-edit="${d.id}" type="button">Изменить</button>`:''}
          ${canDel?`<button class="btn small danger" data-del="${d.id}" type="button">Удалить</button>`:''}
          ${transitions.map(t=>`<button class="btn small workflow" data-trans="${d.id}" data-to="${t.to}" type="button">${esc(t.label)}</button>`).join('')}
        </div>
      </div>`:''}
    </article>`;
  }).join('');
  $('#docsEmpty').hidden = list.length>0;
  $$('#docsList [data-edit]').forEach(b=> b.onclick=()=>openDocModal(b.dataset.edit));
  $$('#docsList [data-del]').forEach(b=> b.onclick=()=>deleteDoc(b.dataset.del));
  $$('#docsList [data-trans]').forEach(b=> b.onclick=()=>transitionDoc(b.dataset.trans, b.dataset.to));
}

$('#searchInput').oninput = renderDocs;
$('#statusFilter').onchange = renderDocs;
$('#addDocBtn').onclick = ()=>openDocModal(null);

function openDocModal(id){
  editingId=id;
  if(id){
    const d=load(KEYS.docs,[]).find(x=>x.id===id);
    if(!d) return;
    $('#modalTitle').textContent='Редактирование документа';
    $('#f_reg').value=d.regNumber; $('#f_type').value=d.type; $('#f_date').value=d.date;
    $('#f_sender').value=d.sender; $('#f_recipient').value=d.recipient;
    $('#f_status').value=d.status; $('#f_note').value=d.note||'';
  }else{
    $('#modalTitle').textContent='Новый документ';
    $('#docForm').reset();
    $('#f_date').value=new Date().toISOString().slice(0,10);
    $('#f_reg').value=nextRegNumber();
    $('#f_status').value='draft';
  }
  $('#f_statusWrap').hidden = currentUser.role!=='admin';
  $('#modal').hidden=false;
  setTimeout(()=>$('#f_reg').focus(),30);
}
function closeModal(){ $('#modal').hidden=true; editingId=null; }
$('#modalClose').onclick=closeModal;
$('#modalCancel').onclick=closeModal;
$('#modal').addEventListener('click', e=>{ if(e.target.id==='modal') closeModal(); });
document.addEventListener('keydown', e=>{
  if(e.key==='Escape'){
    if(!$('#modal').hidden) closeModal();
    if(!$('#resetModal').hidden) closeResetModal();
  }
});

$('#docForm').onsubmit = e=>{
  e.preventDefault();
  const base={
    regNumber:$('#f_reg').value.trim(),
    type:$('#f_type').value,
    date:$('#f_date').value,
    sender:$('#f_sender').value.trim(),
    recipient:$('#f_recipient').value.trim(),
    note:$('#f_note').value.trim()
  };
  if(!base.regNumber||!base.sender||!base.recipient) return toast('Заполните обязательные поля');
  const docs=load(KEYS.docs,[]);
  if(editingId){
    const i=docs.findIndex(d=>d.id===editingId);
    if(i<0) return;
    const status = currentUser.role==='admin' ? $('#f_status').value : docs[i].status;
    docs[i]={...docs[i], ...base, status, updatedAt:Date.now()};
    save(KEYS.docs,docs);
    logAction('Изменение документа', `${base.regNumber} (${base.type})`);
    toast('Документ обновлён');
  }else{
    docs.unshift({...base, status:'draft', id:uid(), createdBy:currentUser.login, createdAt:Date.now(), updatedAt:Date.now()});
    save(KEYS.docs,docs);
    logAction('Создание документа', `${base.regNumber} (${base.type}) — Черновик`);
    toast('Документ добавлен (Черновик)');
  }
  closeModal(); renderDocs();
};

function transitionDoc(id, newStatus){
  const docs=load(KEYS.docs,[]);
  const i=docs.findIndex(d=>d.id===id);
  if(i<0) return;
  const doc=docs[i];
  const allowed = getAvailableTransitions(currentUser, doc);
  if(!allowed.some(t=>t.to===newStatus)){
    return toast('Недостаточно прав для этого действия');
  }
  const old = doc.status;
  doc.status=newStatus;
  doc.updatedAt=Date.now();
  save(KEYS.docs,docs);
  logAction('Смена статуса', `${doc.regNumber}: ${statusLabel(old)} → ${statusLabel(newStatus)}`);
  renderDocs();
  toast('Статус: '+statusLabel(newStatus));
}

function deleteDoc(id){
  const docs=load(KEYS.docs,[]);
  const d=docs.find(x=>x.id===id);
  if(!d) return;
  if(!confirm(`Удалить документ ${d.regNumber}?`)) return;
  save(KEYS.docs, docs.filter(x=>x.id!==id));
  logAction('Удаление документа', `${d.regNumber} (${d.type})`);
  renderDocs(); toast('Документ удалён');
}

/* ================= CSV ================= */
function downloadCSV(filename, rows){
  const csv = rows.map(row=> row.map(cell=>{
    const s=String(cell==null?'':cell);
    return /[;"\n\r]/.test(s) ? '"'+s.replace(/"/g,'""')+'"' : s;
  }).join(';')).join('\r\n');
  const blob = new Blob(['\ufeff'+csv], {type:'text/csv;charset=utf-8;'});
  const url = URL.createObjectURL(blob);
  const a = document.createElement('a');
  a.href=url; a.download=filename;
  document.body.appendChild(a); a.click();
  document.body.removeChild(a);
  URL.revokeObjectURL(url);
}

$('#exportDocsBtn').onclick = ()=>{
  const list = getFilteredDocs();
  if(!list.length){ toast('Нет документов для экспорта'); return; }
  const rows = [['Рег. номер','Тип','Дата','Отправитель','Получатель','Статус','Примечание','Создал','Создан','Обновлён']];
  list.forEach(d=>{
    rows.push([d.regNumber, d.type, d.date, d.sender, d.recipient,
      statusLabel(d.status), d.note||'', d.createdBy, fmt(d.createdAt), fmt(d.updatedAt)]);
  });
  const stamp = new Date().toISOString().slice(0,10);
  downloadCSV(`реестр_документов_${stamp}.csv`, rows);
  logAction('Экспорт CSV','Реестр документов');
  toast('Файл CSV скачан');
};

$('#exportLogBtn').onclick = ()=>{
  const log = load(KEYS.log, []);
  if(!log.length){ toast('Журнал пуст'); return; }
  const rows = [['Время','Пользователь','Роль','Действие','Детали']];
  log.forEach(l=> rows.push([fmt(l.ts), l.login, roleLabel(l.role), l.action, l.details||'']));
  const stamp = new Date().toISOString().slice(0,10);
  downloadCSV(`журнал_действий_${stamp}.csv`, rows);
  logAction('Экспорт CSV','Журнал действий');
  toast('Журнал скачан');
};

/* ================= админ-панель ================= */
function dateKey(ts){
  const d=new Date(ts);
  return d.getFullYear()+'-'+String(d.getMonth()+1).padStart(2,'0')+'-'+String(d.getDate()).padStart(2,'0');
}

function renderDailyChart(){
  const log=load(KEYS.log,[]);
  const days=14;
  const buckets=[];
  const now=new Date(); now.setHours(0,0,0,0);
  for(let i=days-1;i>=0;i--){
    const d=new Date(now); d.setDate(d.getDate()-i);
    buckets.push({key:dateKey(d.getTime()), date:d, count:0});
  }
  const byKey={}; buckets.forEach(b=> byKey[b.key]=b);
  log.forEach(l=>{
    if(l.action!=='Вход') return;
    const k=dateKey(l.ts);
    if(byKey[k]) byKey[k].count++;
  });
  const max=Math.max(1, ...buckets.map(b=>b.count));
  $('#dailyChart').innerHTML = buckets.map(b=>{
    const h = b.count? Math.max(6, Math.round(b.count/max*100)) : 2;
    const lbl = b.date.toLocaleDateString('ru-RU',{day:'2-digit',month:'2-digit'});
    return `<div class="day-col" title="${lbl}: ${b.count}">
      <div class="day-count">${b.count||''}</div>
      <div class="day-bar-wrap"><div class="day-bar" style="height:${h}%"></div></div>
      <div class="day-label">${lbl}</div>
    </div>`;
  }).join('');
}

function renderAdmin(){
  if(!currentUser || currentUser.role!=='admin') return;
  const users=load(KEYS.users,[]);
  const docs=load(KEYS.docs,[]);
  const log=load(KEYS.log,[]);
  const dayStart=new Date(); dayStart.setHours(0,0,0,0);
  const loginsToday=log.filter(l=>l.action==='Вход' && l.ts>=dayStart.getTime()).length;
  const weekAgo=Date.now()-7*864e5;
  const activeUsers=new Set(log.filter(l=>l.ts>=weekAgo && l.login!=='—').map(l=>l.login)).size;

  $('#statGrid').innerHTML = [
    ['Пользователей', users.length],
    ['Документов в реестре', docs.length],
    ['Входов сегодня', loginsToday],
    ['Активных за 7 дней', activeUsers],
    ['Записей в журнале', log.length]
  ].map(([lbl,num])=>`<div class="stat"><div class="num">${num}</div><div class="lbl">${esc(lbl)}</div></div>`).join('');

  renderDailyChart();

  const counts={};
  log.forEach(l=> counts[l.action]=(counts[l.action]||0)+1);
  const entries=Object.entries(counts).sort((a,b)=>b[1]-a[1]).slice(0,8);
  const max=Math.max(1, ...entries.map(e=>e[1]));
  $('#actionChart').innerHTML = entries.length
    ? entries.map(([act,c])=>`
      <div class="chart-row">
        <div title="${esc(act)}">${esc(act)}</div>
        <div class="chart-bar-wrap"><div class="chart-bar" style="width:${Math.round(c/max*100)}%"></div></div>
        <div style="text-align:right;color:var(--muted)">${c}</div>
      </div>`).join('')
    : '<p class="muted small">Пока нет данных</p>';

  $('#usersTable').innerHTML =
    '<thead><tr><th>Логин</th><th>Роль</th><th>Создан</th><th>Последний вход</th><th>Пароль</th></tr></thead><tbody>' +
    users.map(u=>{
      const isSelf = u.id===currentUser.id;
      const options = Object.entries(ROLES).map(([k,v])=>
        `<option value="${k}" ${u.role===k?'selected':''}>${esc(v.label)}</option>`).join('');
      return `<tr>
        <td>${esc(u.login)}${isSelf?' <span class="muted small">(вы)</span>':''}</td>
        <td>${isSelf
          ? `<span class="role-badge ${u.role==='admin'?'admin':''}">${esc(roleLabel(u.role))}</span>`
          : `<select class="role-select" data-role-for="${u.id}">${options}</select>`}</td>
        <td>${fmt(u.createdAt)}</td>
        <td>${fmt(u.lastLogin)}</td>
        <td>${isSelf
          ? '<span class="muted small">—</span>'
          : `<button class="btn small" data-reset-pw="${u.id}" type="button">Сбросить</button>`}</td>
      </tr>`;
    }).join('') + '</tbody>';

  $$('#usersTable [data-role-for]').forEach(sel=>{
    sel.onchange = ()=>{
      const id=sel.dataset.roleFor;
      const newRole=sel.value;
      const users=load(KEYS.users,[]);
      const u=users.find(x=>x.id===id);
      if(!u) return;
      const oldRole=u.role;
      u.role=newRole;
      save(KEYS.users,users);
      logAction('Смена роли', `${u.login}: ${roleLabel(oldRole)} → ${roleLabel(newRole)}`);
      toast(`Роль ${u.login}: ${roleLabel(newRole)}`);
    };
  });

  $$('#usersTable [data-reset-pw]').forEach(btn=>{
    btn.onclick = ()=> openResetModal(btn.dataset.resetPw);
  });

  $('#logTable').innerHTML =
    '<thead><tr><th>Время</th><th>Пользователь</th><th>Действие</th><th>Детали</th></tr></thead><tbody>' +
    (log.slice(0,150).map(l=>`<tr>
      <td>${fmt(l.ts)}</td>
      <td>${esc(l.login)}</td>
      <td>${esc(l.action)}</td>
      <td>${esc(l.details)}</td>
    </tr>`).join('') || '<tr><td colspan="4" class="muted">Журнал пуст</td></tr>') + '</tbody>';
}

/* ================= сброс пароля императором ================= */
function openResetModal(userId){
  const u = load(KEYS.users,[]).find(x=>x.id===userId);
  if(!u) return;
  resetUserId = userId;
  $('#resetUserLabel').textContent = 'Пользователь: ' + u.login;
  $('#resetForm').reset();
  $('#resetModal').hidden = false;
  setTimeout(()=>$('#resetPass').focus(), 30);
}
function closeResetModal(){ $('#resetModal').hidden=true; resetUserId=null; }
$('#resetClose').onclick = closeResetModal;
$('#resetCancel').onclick = closeResetModal;
$('#resetModal').addEventListener('click', e=>{ if(e.target.id==='resetModal') closeResetModal(); });

$('#resetForm').onsubmit = e => {
  e.preventDefault();
  const np = $('#resetPass').value;
  const np2 = $('#resetPass2').value;
  if(np.length<4) return toast('Пароль должен быть не короче 4 символов');
  if(np!==np2) return toast('Пароли не совпадают');
  const users = load(KEYS.users, []);
  const u = users.find(x=>x.id===resetUserId);
  if(!u) return;
  u.passHash = hash(np);
  save(KEYS.users, users);
  logAction('Сброс пароля императором', `Пользователь: ${u.login}`);
  closeResetModal();
  toast('Пароль пользователя сброшен');
};

/* ================= init ================= */
fillSelects();
applyTheme(document.documentElement.hasAttribute('data-theme'));

const session=load(KEYS.session,null);
if(session){
  const u=load(KEYS.users,[]).find(x=>x.id===session.id);
  if(u){ currentUser=u; showApp(); }
  else { localStorage.removeItem(KEYS.session); showAuth(); }
}else{
  showAuth();
}

})();
</script>
</body>
</html>
