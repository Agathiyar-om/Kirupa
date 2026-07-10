<html lang="en">
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width,initial-scale=1,viewport-fit=cover">
<title>Smart Home Dashboard</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link href="https://fonts.googleapis.com/css2?family=Fraunces:wght@600;700&family=Nunito:wght@400;600;700;800&family=JetBrains+Mono:wght@400;700&display=swap" rel="stylesheet">
<script src="https://unpkg.com/mqtt/dist/mqtt.min.js"></script>
<style>
:root{
  --bg1:#a8cdf6; --bg2:#c3ddfb;
  --card:#ffffff; --card-shadow:rgba(30,64,120,0.18);
  --text:#1c2b45; --text2:#8a93a6;
  --accent:#2f6fed; --accent-soft:#e7f0ff;
  --track-off:#d9dfe8; --knob:#ffffff;
  --on:#2ecc71; --on-glow:rgba(46,204,113,.4);
  --off:#e74c3c;
  --warn:#f5a623;
  --border:rgba(30,64,120,0.08);
}
*,*::before,*::after{box-sizing:border-box;margin:0;padding:0}
html{height:100%}
body{
  min-height:100%;
  background:linear-gradient(160deg,var(--bg1),var(--bg2));
  color:var(--text);font-family:'Nunito',sans-serif;
  overflow-x:hidden;
}

::-webkit-scrollbar{width:5px}
::-webkit-scrollbar-track{background:transparent}
::-webkit-scrollbar-thumb{background:var(--accent);border-radius:3px}

/* ══ LOGIN PAGE ══ */
#loginPage{position:fixed;inset:0;z-index:900;display:flex;align-items:center;justify-content:center;padding:20px;background:linear-gradient(160deg,var(--bg1),var(--bg2));}
.lp-wrap{width:100%;max-width:380px;animation:fadeUp .6s cubic-bezier(.16,1,.3,1) both;}
@keyframes fadeUp{from{opacity:0;transform:translateY(16px)}to{opacity:1;transform:translateY(0)}}
.lp-logo{display:block;width:74px;height:74px;border-radius:50%;background:var(--card);box-shadow:0 8px 24px var(--card-shadow);object-fit:cover;margin:0 auto 16px;}
.lp-card{background:var(--card);border-radius:22px;padding:34px 30px;box-shadow:0 10px 34px var(--card-shadow);}
.lp-title{font-family:'Fraunces',serif;font-size:21px;font-weight:700;color:var(--text);text-align:center;margin-bottom:2px;}
.lp-sub{font-size:10px;font-weight:700;color:var(--text2);text-align:center;letter-spacing:3px;text-transform:uppercase;margin-bottom:22px;}
.lp-field label{display:block;font-size:10px;font-weight:800;letter-spacing:2px;color:var(--accent);text-transform:uppercase;margin-bottom:6px;}
.pw-wrap{position:relative;margin-bottom:16px;}
.pw-wrap input{width:100%;background:#f4f7fc;border:1.5px solid var(--border);border-radius:10px;padding:12px 42px 12px 14px;font-size:15px;font-weight:600;color:var(--text);outline:none;transition:border-color .2s;}
.pw-wrap input:focus{border-color:var(--accent);}
.eye-btn{position:absolute;right:11px;top:50%;transform:translateY(-50%);background:none;border:none;cursor:pointer;color:var(--text2);font-size:16px;}
.lp-btn{width:100%;padding:13px;border:none;border-radius:12px;background:var(--accent);font-family:'Fraunces',serif;font-size:14px;font-weight:700;letter-spacing:.5px;color:#fff;cursor:pointer;box-shadow:0 6px 18px rgba(47,111,237,.4);transition:transform .12s,box-shadow .2s;}
.lp-btn:hover{transform:translateY(-1px);box-shadow:0 8px 22px rgba(47,111,237,.5);}
.lp-err{margin-top:12px;padding:9px 14px;background:#fdeceb;border:1px solid #f6c6c1;border-radius:8px;font-size:12px;color:#c0392b;text-align:center;display:none;}
@keyframes shake{0%,100%{transform:translateX(0)}20%,60%{transform:translateX(-7px)}40%,80%{transform:translateX(7px)}}

/* ══ DASHBOARD ══ */
#dashboard{display:none;min-height:100vh;flex-direction:column;}
.hdr{display:flex;align-items:center;justify-content:center;position:relative;padding:22px 16px 6px;}
.hdr-title{font-family:'Fraunces',serif;font-size:22px;font-weight:700;color:var(--text);letter-spacing:.2px;text-align:center;}
.hdr-btns{position:absolute;right:16px;top:20px;display:flex;gap:6px;}
.icon-btn{background:var(--card);border:none;border-radius:9px;padding:7px 10px;color:var(--text2);cursor:pointer;font-size:14px;box-shadow:0 3px 10px var(--card-shadow);transition:all .2s;}
.icon-btn:hover{color:var(--accent);}
.logout-btn{background:var(--card);border:none;border-radius:9px;padding:7px 11px;color:#c0392b;cursor:pointer;font-size:11px;font-weight:800;letter-spacing:1px;box-shadow:0 3px 10px var(--card-shadow);}

.status-row{display:flex;justify-content:center;gap:8px;padding:6px 16px 4px;flex-wrap:wrap;}
.chip{display:flex;align-items:center;gap:5px;background:var(--card);border-radius:20px;padding:5px 12px;font-size:11px;font-weight:700;color:var(--text2);box-shadow:0 3px 10px var(--card-shadow);position:relative;}
.chip:hover .chip-tip{display:block}
.chip-tip{display:none;position:absolute;top:calc(100%+7px);left:50%;transform:translateX(-50%);background:var(--card);border-radius:10px;padding:10px 14px;width:210px;font-size:10px;color:var(--text2);z-index:400;line-height:1.8;box-shadow:0 8px 24px var(--card-shadow);}
.chip-tip b{color:var(--text);display:block;margin-bottom:4px;}
.chip-tip .tr{display:flex;gap:7px;align-items:center;}
.chip-tip .td{width:7px;height:7px;border-radius:50%;flex-shrink:0;}
.dot{width:7px;height:7px;border-radius:50%;flex-shrink:0;transition:background .4s;}
.dot.grey{background:#c3cbd6}.dot.green{background:var(--on);box-shadow:0 0 6px var(--on-glow)}
.dot.amber{background:var(--warn)}.dot.red{background:var(--off)}
.dot.pulse{animation:dp 1.2s ease-in-out infinite}
@keyframes dp{0%,100%{opacity:1}50%{opacity:.3}}

.ctrl{display:flex;gap:8px;padding:10px 16px 4px;max-width:900px;margin:0 auto;width:100%;}
.ctrl-btn{flex:1;padding:9px 4px;border-radius:12px;border:none;font-size:11px;font-weight:800;letter-spacing:.8px;text-transform:uppercase;cursor:pointer;background:var(--card);box-shadow:0 3px 10px var(--card-shadow);color:var(--text);transition:transform .12s;}
.ctrl-btn:hover{transform:translateY(-1px);}
.ctrl-on{color:#219653}.ctrl-off{color:#c0392b}.ctrl-sch{color:var(--accent)}

.grid{
  flex:1;display:grid;grid-template-columns:repeat(4,1fr);gap:14px;
  padding:6px 16px 6px;max-width:900px;margin:0 auto;width:100%;
}
.feature-row{
  display:grid;grid-template-columns:1fr;gap:14px;
  padding:14px 16px 0;max-width:900px;margin:0 auto;width:100%;
}
@media(max-width:640px){.grid{grid-template-columns:repeat(2,1fr);gap:10px;padding:6px 12px;}.feature-row{padding:12px 12px 0;gap:10px;}}

.pagination{display:flex;align-items:center;justify-content:center;gap:16px;padding:8px 16px 30px;}
.page-btn{background:var(--card);border:none;border-radius:20px;padding:8px 14px;display:flex;align-items:center;gap:6px;color:var(--accent);cursor:pointer;box-shadow:0 3px 10px var(--card-shadow);font-size:12px;font-weight:800;letter-spacing:.3px;transition:opacity .2s,transform .12s;flex-shrink:0;}
.page-btn:disabled{opacity:.3;cursor:default;}
.page-btn:hover:not(:disabled){transform:translateY(-1px);}
.page-btn svg{width:16px;height:16px;flex-shrink:0;}
.page-scroller{width:110px;height:6px;background:#d9dfe8;border-radius:3px;position:relative;cursor:pointer;flex-shrink:0;}
.page-scroller-thumb{position:absolute;top:0;left:0;height:100%;background:var(--accent);border-radius:3px;transition:left .25s;cursor:grab;}
.page-scroller-thumb:active{cursor:grabbing;}

/* Today + Water tank feature cards */
.feature-card{background:var(--card);border-radius:18px;padding:16px 18px;box-shadow:0 6px 20px var(--card-shadow);display:flex;flex-direction:column;justify-content:space-between;min-height:150px;}
.feat-title{font-family:'Fraunces',serif;font-size:16px;font-weight:700;color:var(--text);margin-bottom:8px;}
.today-date{font-size:12px;color:var(--text2);font-weight:700;margin-bottom:6px;}
.today-cond{display:flex;align-items:center;gap:8px;font-size:15px;font-weight:800;color:var(--text);margin-bottom:10px;}
.today-cond .wicon{font-size:22px;}
.today-stats{display:flex;gap:26px;}
.today-stat b{display:block;font-size:16px;font-weight:800;color:var(--text);}
.today-stat span{font-size:10px;color:var(--text2);font-weight:700;letter-spacing:.5px;text-transform:uppercase;}

.tank-gauge-wrap{display:flex;flex-direction:column;align-items:center;}
.tank-pct{font-size:20px;font-weight:800;color:var(--text);margin-top:-38px;}
.tank-labels{display:flex;justify-content:space-between;width:100%;font-size:10px;font-weight:800;color:var(--text2);letter-spacing:.5px;text-transform:uppercase;margin-top:2px;}

/* Device cards */
.card{
  background:var(--card);border-radius:18px;padding:14px;display:flex;flex-direction:column;justify-content:space-between;
  min-height:110px;box-shadow:0 6px 18px var(--card-shadow);cursor:pointer;transition:transform .12s,box-shadow .2s;position:relative;
}
.card:hover{transform:translateY(-2px);box-shadow:0 10px 24px var(--card-shadow);}
.card:active{transform:scale(.97);}
.card-top{display:flex;align-items:flex-start;justify-content:space-between;}
.card-name{font-size:15px;font-weight:800;color:var(--text);letter-spacing:.2px;line-height:1.25;max-width:70%;}
.card-icon{width:26px;height:26px;flex-shrink:0;color:#c3cbd6;transition:color .3s;transform-origin:50% 50%;}
.card-icon svg{width:100%;height:100%;}
.card-icon.on-generic{color:var(--accent);}
.card-icon.on-bulb{color:#f5a623;animation:bulbGlow 2s ease-in-out infinite;}
.card-icon.on-fan{color:var(--accent);animation:iconSpin 1s linear infinite;}
.card-icon.on-tap{color:var(--accent);animation:iconPulse 1s ease-in-out infinite;}
.card-icon.on-flame{color:#ff6b35;animation:iconFlicker 1.2s ease-in-out infinite;}
@keyframes bulbGlow{0%,100%{filter:drop-shadow(0 0 3px rgba(245,166,35,.5))}50%{filter:drop-shadow(0 0 7px rgba(245,166,35,.95))}}
@keyframes iconSpin{from{transform:rotate(0deg)}to{transform:rotate(360deg)}}
@keyframes iconPulse{0%,100%{transform:scale(1)}50%{transform:scale(1.18)}}
@keyframes iconFlicker{0%,100%{opacity:1;filter:drop-shadow(0 0 3px rgba(255,107,53,.5))}45%{opacity:.65}50%{opacity:1;filter:drop-shadow(0 0 7px rgba(255,107,53,.9))}75%{opacity:.85}}
.conn-dot{position:absolute;top:8px;left:8px;width:6px;height:6px;border-radius:50%;background:#d9dfe8;transition:background .4s;}
.conn-dot.online{background:var(--on);}
.conn-dot.offline{background:var(--off);}

/* iOS-style switch */
.switch{width:44px;height:25px;border-radius:14px;background:var(--track-off);position:relative;cursor:pointer;transition:background .25s;flex-shrink:0;align-self:flex-start;margin-top:8px;}
.switch.on{background:var(--accent);}
.switch .knob{position:absolute;top:2.5px;left:2.5px;width:20px;height:20px;border-radius:50%;background:var(--knob);box-shadow:0 2px 5px rgba(0,0,0,.25);transition:left .25s;}
.switch.on .knob{left:21.5px;}

/* ══ SCHEDULE PANEL ══ */
.panel-overlay{display:none;position:fixed;inset:0;z-index:500;background:rgba(20,30,50,.35);backdrop-filter:blur(4px);}
.panel-overlay.open{display:block}
.panel{position:fixed;bottom:0;left:0;right:0;z-index:501;background:var(--card);border-radius:20px 20px 0 0;padding:0 0 env(safe-area-inset-bottom,0);max-height:88vh;transform:translateY(100%);transition:transform .35s cubic-bezier(.16,1,.3,1);display:flex;flex-direction:column;box-shadow:0 -8px 30px var(--card-shadow);}
.panel-overlay.open .panel{transform:translateY(0)}
.panel-drag{width:40px;height:4px;border-radius:2px;background:var(--border);margin:10px auto 0;}
.panel-head{display:flex;align-items:center;justify-content:space-between;padding:12px 18px 10px;border-bottom:1px solid var(--border);flex-shrink:0;}
.panel-title{font-family:'Fraunces',serif;font-size:15px;font-weight:700;color:var(--text);}
.panel-close{background:#fdeceb;border:none;border-radius:8px;padding:5px 10px;color:#c0392b;cursor:pointer;font-size:12px;font-weight:800;}
.panel-body{overflow-y:auto;flex:1;padding:12px 16px 18px;}

.sch-block{margin-bottom:14px;background:#f4f7fc;border-radius:14px;overflow:hidden;}
.sch-block-head{display:flex;align-items:center;gap:8px;padding:10px 14px;background:#eaf1fc;}
.sch-block-icon{width:16px;height:16px;color:var(--accent);}
.sch-block-icon svg{width:100%;height:100%;}
.sch-block-name{font-size:13px;font-weight:800;color:var(--text);flex:1;}
.sch-block-body{padding:10px 12px;}
.sch-entries{display:flex;flex-direction:column;gap:5px;margin-bottom:8px;}
.sch-entry{display:flex;align-items:center;gap:6px;background:#fff;border-radius:8px;padding:6px 10px;}
.sdot{width:6px;height:6px;border-radius:50%;flex-shrink:0;}
.sdot.on{background:var(--on);}.sdot.off{background:var(--off);}
.se-txt{flex:1;font-size:11px;color:var(--text);}
.se-day{color:var(--accent);font-size:10px;font-weight:800;}.se-time{font-family:'JetBrains Mono',monospace;font-size:10px;}.se-act{font-size:10px;font-weight:800;}
.se-act.on{color:#219653}.se-act.off{color:#c0392b}
.se-del{background:#fdeceb;border-radius:5px;width:20px;height:20px;display:flex;align-items:center;justify-content:center;cursor:pointer;color:#c0392b;font-size:10px;flex-shrink:0;}
.sch-none{font-size:11px;color:var(--text2);font-style:italic;padding:2px 0 6px;}
.sch-add-lbl{font-size:10px;font-weight:800;letter-spacing:1.5px;color:var(--text2);text-transform:uppercase;margin-bottom:6px;}
.sch-form{display:grid;grid-template-columns:1fr 1fr;gap:6px;}
.sch-form select,.sch-form input[type=time],.sch-form .sch-btn{background:#fff;border:1.5px solid var(--border);border-radius:8px;padding:7px 8px;font-size:12px;font-weight:600;color:var(--text);outline:none;-webkit-appearance:none;appearance:none;}
.sch-btn{grid-column:span 2;background:var(--accent-soft);color:var(--accent);font-weight:800;letter-spacing:.5px;text-transform:uppercase;cursor:pointer;text-align:center;border:none !important;}

/* ══ SETTINGS MODAL ══ */
.modal-overlay{display:none;position:fixed;inset:0;z-index:600;background:rgba(20,30,50,.35);backdrop-filter:blur(6px);align-items:flex-end;justify-content:center;}
.modal-overlay.open{display:flex}
.modal{background:var(--card);border-radius:20px 20px 0 0;padding:22px 20px calc(env(safe-area-inset-bottom,0)+20px);width:100%;max-width:460px;box-shadow:0 -8px 30px var(--card-shadow);}
.modal-title{font-family:'Fraunces',serif;font-size:16px;font-weight:700;color:var(--text);margin-bottom:18px;display:flex;align-items:center;justify-content:space-between;}
.modal-close{background:#fdeceb;border:none;border-radius:8px;padding:4px 10px;color:#c0392b;cursor:pointer;font-size:13px;}
.mf{margin-bottom:12px}
.mf label{display:block;font-size:10px;font-weight:800;letter-spacing:2px;color:var(--accent);text-transform:uppercase;margin-bottom:5px;}
.mf input{width:100%;background:#f4f7fc;border:1.5px solid var(--border);border-radius:10px;padding:10px 12px;font-size:13px;font-weight:600;color:var(--text);outline:none;}
.mf .hint{font-size:10px;color:var(--text2);margin-top:4px;font-style:italic;}
.modal-save{width:100%;margin-top:14px;padding:12px;background:var(--accent);border:none;border-radius:12px;font-family:'Fraunces',serif;font-size:14px;font-weight:700;color:#fff;cursor:pointer;box-shadow:0 6px 18px rgba(47,111,237,.4);}

/* ══ TOAST ══ */
#toast{position:fixed;bottom:20px;left:50%;transform:translateX(-50%);z-index:999;display:flex;flex-direction:column;align-items:center;gap:6px;pointer-events:none;width:calc(100% - 32px);max-width:340px;}
.toast-msg{background:var(--text);color:#fff;border-radius:10px;padding:10px 16px;font-size:13px;font-weight:700;box-shadow:0 8px 24px var(--card-shadow);animation:tIn .3s cubic-bezier(.16,1,.3,1) both,tOut .3s ease .22s forwards;display:flex;align-items:center;gap:7px;width:100%;}
@keyframes tIn{from{opacity:0;transform:translateY(8px)}to{opacity:1;transform:translateY(0)}}
@keyframes tOut{from{opacity:1}to{opacity:0}}
</style>
</head>
<body>

<!-- ══ LOGIN PAGE ══ -->
<div id="loginPage">
  <div class="lp-wrap">
    <img class="lp-logo" id="loginLogo" src="" alt="Home">
    <div class="lp-card">
      <div class="lp-title">Smart Home</div>
      <div class="lp-sub">Secure Access Portal</div>
      <div class="lp-field">
        <label>Access Password</label>
        <div class="pw-wrap">
          <input type="password" id="pwInput" placeholder="Enter password…" autocomplete="current-password">
          <button class="eye-btn" id="eyeBtn" tabindex="-1">👁</button>
        </div>
      </div>
      <button class="lp-btn" id="loginBtn">Enter Home</button>
      <div class="lp-err" id="loginErr">Incorrect password. Try again.</div>
    </div>
  </div>
</div>

<!-- ══ DASHBOARD ══ -->
<div id="dashboard">
  <div class="hdr">
    <div class="hdr-title">Smart Home Dashboard</div>
    <div class="hdr-btns">
      <button class="icon-btn" id="settingsBtn" title="Settings">⚙</button>
      <button class="logout-btn" id="logoutBtn">OUT</button>
    </div>
  </div>

  <div class="status-row">
    <div class="chip"><span>📶</span><span id="speedVal">--</span><span style="font-size:9px">M/s</span></div>
    <div class="chip" id="connChip">
      <span>🔌</span><span id="connCountVal">0/14</span>
      <div class="chip-tip"><b>Devices Connected</b>
        <div class="tr"><span class="td" style="background:var(--on)"></span>Reported state since connect</div>
        <div class="tr"><span class="td" style="background:var(--off)"></span>Not reported yet / offline</div>
      </div>
    </div>
    <div class="chip" id="espChip">
      <div class="dot grey pulse" id="espDot"></div><span id="espLbl">ESP32</span>
      <div class="chip-tip"><b>ESP32 Live Status</b>
        <div class="tr"><span class="td" style="background:var(--on)"></span>Heartbeat &lt;40s ago</div>
        <div class="tr"><span class="td" style="background:var(--warn)"></span>No heartbeat 40–90s</div>
        <div class="tr"><span class="td" style="background:var(--off)"></span>Offline / &gt;90s</div>
      </div>
    </div>
    <div class="chip"><div class="dot grey" id="mqttDot"></div><span id="mqttLbl">MQTT</span></div>
  </div>

  <div class="ctrl">
    <button class="ctrl-btn ctrl-sch" onclick="openSchedules()">Schedules</button>
  </div>

  <div class="feature-row" id="featureRow"></div>
  <div class="grid" id="appGrid"></div>
  <div class="pagination" id="pagination"></div>
</div>

<!-- ══ SCHEDULE PANEL ══ -->
<div class="panel-overlay" id="panelOverlay">
  <div class="panel" id="schedPanel">
    <div class="panel-drag"></div>
    <div class="panel-head">
      <span class="panel-title">Schedules</span>
      <button class="panel-close" onclick="closeSchedules()">CLOSE</button>
    </div>
    <div class="panel-body" id="panelBody"></div>
  </div>
</div>

<!-- ══ SETTINGS MODAL ══ -->
<div class="modal-overlay" id="settingsModal">
  <div class="modal">
    <div class="modal-title">MQTT Settings<button class="modal-close" id="mClose">✕</button></div>
    <div class="mf"><label>Broker Host</label><input id="cfgHost" placeholder="broker.emqx.io"></div>
    <div class="mf"><label>WebSocket Port</label><input id="cfgPort" type="number" placeholder="8084"><div class="hint">Web uses WSS (8084). ESP32 uses plain MQTT (1883).</div></div>
    <div class="mf"><label>Topic Prefix</label><input id="cfgPrefix" placeholder="HOME_AUTO"><div class="hint">Must match MQTT_PREFIX in sketch.</div></div>
    <div class="mf"><label>Username (optional)</label><input id="cfgUser" placeholder="leave blank"></div>
    <div class="mf"><label>Password (optional)</label><input id="cfgPass" type="password" placeholder="leave blank"></div>
    <button class="modal-save" id="cfgSave">SAVE &amp; RECONNECT</button>
  </div>
</div>

<div id="toast"></div>

<script>
/* ── LOGO ── */
const LOGO_SRC='data:image/svg+xml;utf8,'+encodeURIComponent(`
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 100 100">
  <rect width="100" height="100" fill="#ffffff"/>
  <path d="M50 18 L84 46 H76 V82 H24 V46 H16 Z" fill="none" stroke="#2f6fed" stroke-width="5" stroke-linejoin="round"/>
  <rect x="33" y="53" width="15" height="15" fill="#e7f0ff"/>
  <rect x="52" y="53" width="15" height="15" fill="#2f6fed"/>
  <rect x="43" y="70" width="14" height="12" fill="#2f6fed"/>
</svg>`);
document.getElementById('loginLogo').src=LOGO_SRC;

/* ── ICONS (inline SVG, stroke=currentColor) ── */
const ICONS={
  bulb:'<svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round"><path d="M9 18h6M10 21h4M12 3a6 6 0 0 0-3.5 10.9c.6.4 1 1.1 1 1.9v.2h5v-.2c0-.8.4-1.5 1-1.9A6 6 0 0 0 12 3Z"/></svg>',
  fan:'<svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round"><circle cx="12" cy="12" r="1.6" fill="currentColor" stroke="none"/><path d="M12 12c0-3.5 1.5-7 4-7 2 0 3 1.5 3 3 0 3-3.5 4-7 4Z"/><path d="M12 12c-3.5 0-7-1.5-7-4 0-2 1.5-3 3-3 3 0 4 3.5 4 7Z"/><path d="M12 12c3.5 0 7 1.5 7 4 0 2-1.5 3-3 3-3 0-4-3.5-4-7Z"/></svg>',
  tv:'<svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round"><rect x="3" y="6" width="18" height="12" rx="2"/><path d="M8 21h8M12 18v3"/></svg>',
  flame:'<svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round"><path d="M12 2c1 3-2 4-2 7a4 4 0 0 0 8 0c0-1-.5-2-1-2.5.5 2-1 3-1 3 .5-2.5-2-3.5-2-5.5C14 3 13 2.3 12 2Zm-4 12a4 4 0 1 0 8 0c0-2-2-3-4-7-2 4-4 5-4 7Z"/></svg>',
  tap:'<svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round"><path d="M4 10V7a3 3 0 0 1 3-3h9a2 2 0 0 1 2 2v1"/><path d="M13 10v2a3 3 0 0 1-3 3H8"/><path d="M8 15v2a3 3 0 0 0 6 0"/><circle cx="4" cy="10" r="1.4" fill="currentColor" stroke="none"/></svg>',
  door:'<svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round"><rect x="4" y="3" width="16" height="18" rx="1"/><path d="M9 12h.01"/></svg>',
  washer:'<svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round"><rect x="4" y="3" width="16" height="18" rx="2"/><circle cx="12" cy="13" r="4.5"/><path d="M7 6h.01M10 6h.01"/></svg>',
  leaf:'<svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round"><path d="M20 4C10 4 4 10 4 18c8 0 14-6 14-14Z"/><path d="M5 19c4-4 8-8 14-14"/></svg>',
};

/* ── APPLIANCES (14 home devices) ── */
const APPS=[
  {id:1,  name:'Living Room',    icon:'bulb'},
  {id:2,  name:'Bed Room',       icon:'bulb'},
  {id:3,  name:'Kitchen Room',   icon:'bulb'},
  {id:4,  name:'Hall Light',     icon:'bulb'},
  {id:5,  name:'Porch Light',    icon:'bulb'},
  {id:6,  name:'Balcony Light',  icon:'bulb'},
  {id:7,  name:'Living Room',    icon:'fan'},
  {id:8,  name:'Dinning Room',   icon:'fan'},
  {id:9,  name:'TV / Set-top',   icon:'tv'},
  {id:10, name:'Water Heater',   icon:'flame'},
  {id:11, name:'Water Pump',     icon:'tap'},
  {id:12, name:'Garage Door',    icon:'door'},
  {id:13, name:'Washing Machine',icon:'washer'},
  {id:14, name:'Garden Sprinkler',icon:'leaf'},
];
const DAY_OPTS=[
  {v:'DAILY',l:'Every Day'},{v:'SUN',l:'Sun'},{v:'MON',l:'Mon'},
  {v:'TUE',l:'Tue'},{v:'WED',l:'Wed'},{v:'THU',l:'Thu'},
  {v:'FRI',l:'Fri'},{v:'SAT',l:'Sat'},
];
const WEATHER_MAP={ /* WMO code -> [emoji,label] */
  0:['☀️','Clear'],1:['🌤','Mostly Clear'],2:['⛅','Partly Cloudy'],3:['☁️','Cloudy'],
  45:['🌫','Fog'],48:['🌫','Fog'],51:['🌦','Light Drizzle'],53:['🌦','Drizzle'],55:['🌧','Drizzle'],
  61:['🌧','Light Rain'],63:['🌧','Rain'],65:['🌧','Heavy Rain'],71:['🌨','Snow'],73:['🌨','Snow'],
  75:['❄️','Heavy Snow'],80:['🌦','Showers'],81:['🌧','Showers'],82:['⛈','Violent Showers'],
  95:['⛈','Thunderstorm'],96:['⛈','Thunderstorm'],99:['⛈','Thunderstorm'],
};

/* ── ICON ANIMATION STATE ── */
function iconStateClass(iconType,on){
  if(!on)return'';
  if(iconType==='bulb')return'on-bulb';
  if(iconType==='fan')return'on-fan';
  if(iconType==='tap')return'on-tap';
  if(iconType==='flame')return'on-flame';
  return'on-generic';
}

/* ── STATE ── */
let relayState=Array(APPS.length).fill(false);
let deviceOnline=Array(APPS.length).fill(false);
let tankLevel=0;
let schedules=JSON.parse(localStorage.getItem('home_sch_v1')||'{}');
let mqttClient=null;
let cfg=loadCfg();
let lastHbTime=0;
let hbTimer=null;

function loadCfg(){return{host:localStorage.getItem('mq_host')||'broker.emqx.io',port:localStorage.getItem('mq_port')||'8084',prefix:localStorage.getItem('mq_pfx')||'HOME_AUTO',user:localStorage.getItem('mq_user')||'',pass:localStorage.getItem('mq_pass')||''};}
function saveCfg(c){localStorage.setItem('mq_host',c.host);localStorage.setItem('mq_port',c.port);localStorage.setItem('mq_pfx',c.prefix);localStorage.setItem('mq_user',c.user);localStorage.setItem('mq_pass',c.pass);cfg=c;}
function saveSch(){localStorage.setItem('home_sch_v1',JSON.stringify(schedules));}

/* ── LOGIN (default password: home123) ── */
async function sha256(t){const b=await crypto.subtle.digest('SHA-256',new TextEncoder().encode(t));return[...new Uint8Array(b)].map(x=>x.toString(16).padStart(2,'0')).join('');}
const PWD='home123';let HASH='';sha256(PWD).then(h=>HASH=h);
async function doLogin(){
  const pw=document.getElementById('pwInput').value;
  if(!pw){shake();return;}
  if(await sha256(pw)===HASH){sessionStorage.setItem('hauth','1');openDashboard();}
  else{document.getElementById('loginErr').style.display='block';shake();setTimeout(()=>document.getElementById('loginErr').style.display='none',3500);}
}
function shake(){const el=document.getElementById('pwInput');el.style.animation='none';el.offsetWidth;el.style.animation='shake .4s ease';}
document.getElementById('loginBtn').addEventListener('click',doLogin);
document.getElementById('pwInput').addEventListener('keydown',e=>e.key==='Enter'&&doLogin());
document.getElementById('eyeBtn').addEventListener('click',()=>{const i=document.getElementById('pwInput');i.type=i.type==='password'?'text':'password';});
if(sessionStorage.getItem('hauth')==='1'){document.getElementById('loginPage').style.display='none';openDashboard();}

/* ── DASHBOARD ── */
function openDashboard(){
  document.getElementById('loginPage').style.display='none';
  document.getElementById('dashboard').style.display='flex';
  document.getElementById('dashboard').style.flexDirection='column';
  buildGrid();connectMQTT();startSpeed();startClock();fetchWeather();
  if(hbTimer)clearInterval(hbTimer);
  hbTimer=setInterval(updateESPDot,5000);
  updateESPDot();updateConnCount();
}

/* ── CLOCK / DATE ── */
function startClock(){updateClock();setInterval(updateClock,1000);}
function updateClock(){
  const el=document.getElementById('todayDate');if(!el)return;
  const d=new Date();
  const dateStr=d.toLocaleDateString('en-GB',{day:'2-digit',month:'2-digit',year:'numeric'});
  const timeStr=d.toLocaleTimeString('en-US',{hour:'numeric',minute:'2-digit',hour12:true});
  el.textContent=`${dateStr}   ${timeStr}`;
}

/* ── WEATHER (Open-Meteo, no key) ── */
async function fetchWeather(){
  try{
    const r=await fetch('https://api.open-meteo.com/v1/forecast?latitude=9.9252&longitude=78.1198&current=temperature_2m,relative_humidity_2m,weather_code&timezone=Asia%2FKolkata');
    const d=await r.json();
    const c=d.current;
    const w=WEATHER_MAP[c.weather_code]||['🌡','—'];
    document.getElementById('wIcon').textContent=w[0];
    document.getElementById('wCond').textContent=w[1];
  }catch(e){
    document.getElementById('wCond').textContent='Unavailable';
  }
}

/* ── PAGINATION ── */
const PAGE_SIZE=14;
let currentPage=0;
const totalPages=Math.ceil(APPS.length/PAGE_SIZE);

const ICON_PREV='<svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.2" stroke-linecap="round" stroke-linejoin="round"><path d="M15 18l-6-6 6-6"/></svg>';
const ICON_NEXT='<svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.2" stroke-linecap="round" stroke-linejoin="round"><path d="M9 18l6-6-6-6"/></svg>';

function buildFeatureCards(){
  const f=document.getElementById('featureRow');f.innerHTML='';

  const today=document.createElement('div');
  today.className='feature-card';
  today.innerHTML=`
    <div class="feat-title">Today</div>
    <div class="today-date" id="todayDate">--</div>
    <div class="today-cond"><span class="wicon" id="wIcon">⛅</span><span id="wCond">Loading…</span></div>`;
  f.appendChild(today);
}

function renderDevicePage(){
  const g=document.getElementById('appGrid');g.innerHTML='';
  const start=currentPage*PAGE_SIZE;
  const pageApps=APPS.slice(start,start+PAGE_SIZE);
  pageApps.forEach(a=>{
    const s=relayState[a.id-1];
    const d=document.createElement('div');
    d.className='card';d.id='card'+a.id;
    d.innerHTML=`
      <span class="conn-dot" id="conn${a.id}"></span>
      <div class="card-top">
        <span class="card-name">${a.name}</span>
        <span class="card-icon ${iconStateClass(a.icon,s)}" id="icon${a.id}">${ICONS[a.icon]}</span>
      </div>
      <div class="switch ${s?'on':''}" id="tog${a.id}"><div class="knob"></div></div>`;
    d.addEventListener('click',()=>toggle(a.id));
    g.appendChild(d);
  });
  pageApps.forEach(a=>updateConnDot(a.id-1));
  renderPagination();
}

function renderPagination(){
  const p=document.getElementById('pagination');p.innerHTML='';
  if(totalPages<=1){p.style.display='none';return;}
  p.style.display='flex';
  const prevBtn=document.createElement('button');
  prevBtn.className='page-btn';prevBtn.innerHTML=ICON_PREV+'<span>Previous</span>';
  prevBtn.disabled=currentPage===0;
  prevBtn.title='Previous page';
  prevBtn.addEventListener('click',()=>goToPage(currentPage-1));
  p.appendChild(prevBtn);

  const scroller=document.createElement('div');scroller.className='page-scroller';scroller.id='pageScroller';
  const thumb=document.createElement('div');thumb.className='page-scroller-thumb';thumb.id='pageScrollerThumb';
  scroller.appendChild(thumb);
  p.appendChild(scroller);
  layoutScroller();
  scroller.addEventListener('click',e=>{
    if(e.target===thumb)return;
    const rect=scroller.getBoundingClientRect();
    const ratio=(e.clientX-rect.left)/rect.width;
    goToPage(Math.min(totalPages-1,Math.max(0,Math.floor(ratio*totalPages))));
  });
  let dragging=false;
  const onMove=x=>{
    const rect=scroller.getBoundingClientRect();
    const ratio=(x-rect.left)/rect.width;
    goToPage(Math.min(totalPages-1,Math.max(0,Math.floor(ratio*totalPages))));
  };
  thumb.addEventListener('mousedown',()=>{dragging=true;});
  window.addEventListener('mousemove',e=>{if(dragging)onMove(e.clientX);});
  window.addEventListener('mouseup',()=>{dragging=false;});
  thumb.addEventListener('touchstart',()=>{dragging=true;},{passive:true});
  window.addEventListener('touchmove',e=>{if(dragging&&e.touches[0])onMove(e.touches[0].clientX);},{passive:true});
  window.addEventListener('touchend',()=>{dragging=false;});

  const nextBtn=document.createElement('button');
  nextBtn.className='page-btn';nextBtn.innerHTML='<span>Next</span>'+ICON_NEXT;
  nextBtn.disabled=currentPage===totalPages-1;
  nextBtn.title='Next page';
  nextBtn.addEventListener('click',()=>goToPage(currentPage+1));
  p.appendChild(nextBtn);
}
function layoutScroller(){
  const thumb=document.getElementById('pageScrollerThumb');if(!thumb)return;
  const w=100/totalPages;
  thumb.style.width=w+'%';
  thumb.style.left=(currentPage*w)+'%';
}
function goToPage(p){
  if(p<0||p>=totalPages||p===currentPage)return;
  currentPage=p;renderDevicePage();
}

/* ── GRID ── */
function buildGrid(){
  buildFeatureCards();
  renderDevicePage();
}
function updateCard(idx){
  const a=APPS[idx],s=relayState[idx];
  const sw=document.getElementById('tog'+a.id);if(!sw)return;
  sw.className='switch '+(s?'on':'');
  const icon=document.getElementById('icon'+a.id);
  if(icon)icon.className='card-icon '+iconStateClass(a.icon,s);
}
function updateAllCards(){relayState.forEach((_,i)=>updateCard(i));}

function updateConnDot(idx){
  const a=APPS[idx];
  const dot=document.getElementById('conn'+a.id);if(!dot)return;
  dot.className='conn-dot '+(deviceOnline[idx]?'online':'offline');
}
function updateConnCount(){
  const count=deviceOnline.filter(Boolean).length;
  const el=document.getElementById('connCountVal');
  if(el)el.textContent=`${count}/${APPS.length}`;
}
function setAllDevicesOffline(){deviceOnline=Array(APPS.length).fill(false);deviceOnline.forEach((_,i)=>updateConnDot(i));updateConnCount();}

function updateTankGauge(){
  const arc=document.getElementById('tankArc');if(!arc)return;
  const pct=Math.max(0,Math.min(100,tankLevel));
  const len=188.5;
  arc.style.strokeDashoffset=len-(len*pct/100);
  document.getElementById('tankPct').textContent=pct+'%';
}

/* ── RELAY ── */
function toggle(appId){const idx=appId-1,ns=!relayState[idx];relayState[idx]=ns;updateCard(idx);mqttPub(`${cfg.prefix}/relay/${appId}/cmd`,ns?'ON':'OFF');toast(`${APPS[idx].name} → ${ns?'ON':'OFF'}`,ns?'🟢':'🔴');}
function allOn(){for(let i=0;i<APPS.length;i++){relayState[i]=true;updateCard(i);mqttPub(`${cfg.prefix}/relay/${i+1}/cmd`,'ON');}toast(`${APPS.length} Devices → ON`,'✅');}
function allOff(){for(let i=0;i<APPS.length;i++){relayState[i]=false;updateCard(i);mqttPub(`${cfg.prefix}/relay/${i+1}/cmd`,'OFF');}toast(`${APPS.length} Devices → OFF`,'❌');}

/* ── SCHEDULE PANEL ── */
function openSchedules(){buildPanel();document.getElementById('panelOverlay').classList.add('open');}
function closeSchedules(){document.getElementById('panelOverlay').classList.remove('open');}
document.getElementById('panelOverlay').addEventListener('click',e=>{if(e.target.id==='panelOverlay')closeSchedules();});

function buildPanel(){
  const body=document.getElementById('panelBody');body.innerHTML='';
  APPS.forEach(a=>{
    const list=(schedules[a.id]||[]);
    const entries=list.length?list.map((s,i)=>`
      <div class="sch-entry">
        <span class="sdot ${s.action==='ON'?'on':'off'}"></span>
        <span class="se-txt"><span class="se-day">${s.day}</span> <span class="se-time">${s.time}</span> <span class="se-act ${s.action==='ON'?'on':'off'}">${s.action}</span></span>
        <div class="se-del" onclick="delSch(${a.id},${i})">✕</div>
      </div>`).join(''):'<div class="sch-none">— no schedules —</div>';
    const dayOpts=DAY_OPTS.map(d=>`<option value="${d.v}">${d.l}</option>`).join('');
    const block=document.createElement('div');block.className='sch-block';
    block.innerHTML=`
      <div class="sch-block-head">
        <span class="sch-block-icon">${ICONS[a.icon]}</span>
        <span class="sch-block-name">${a.name}</span>
        <span style="font-size:10px;color:var(--text2)">${list.length} set</span>
      </div>
      <div class="sch-block-body">
        <div class="sch-entries">${entries}</div>
        <div class="sch-add-lbl">Add Schedule</div>
        <div class="sch-form">
          <select id="sd${a.id}">${dayOpts}</select>
          <input type="time" id="st${a.id}" value="06:00">
          <select id="sa${a.id}"><option value="ON">Turn ON</option><option value="OFF">Turn OFF</option></select>
          <button class="sch-btn" onclick="addSch(${a.id})">+ ADD</button>
        </div>
      </div>`;
    body.appendChild(block);
  });
}
function addSch(appId){
  const day=document.getElementById('sd'+appId).value,time=document.getElementById('st'+appId).value,action=document.getElementById('sa'+appId).value;
  if(!time){toast('Set a valid time','⚠');return;}
  if(!schedules[appId])schedules[appId]=[];
  schedules[appId].push({day,time,action});saveSch();
  mqttPub(`${cfg.prefix}/schedule/set`,JSON.stringify({relay:appId,day,time,action}));
  toast(`${APPS[appId-1].name} ${action} @ ${time} (${day})`,'⏰');buildPanel();
}
function delSch(appId,idx){schedules[appId].splice(idx,1);saveSch();buildPanel();toast('Schedule removed','🗑');}

/* ── MQTT ── */
function connectMQTT(){
  if(mqttClient&&mqttClient.connected)mqttClient.end(true);
  lastHbTime=0;updateESPDot();setMQTT('connecting');setAllDevicesOffline();
  const url=`wss://${cfg.host}:${cfg.port}/mqtt`;
  const cid='home_web_'+Math.random().toString(36).substring(2,9);
  const opts={clientId:cid,clean:true,reconnectPeriod:5000,connectTimeout:15000};
  if(cfg.user){opts.username=cfg.user;opts.password=cfg.pass;}
  mqttClient=mqtt.connect(url,opts);

  mqttClient.on('connect',()=>{
    setMQTT('connected');toast('MQTT Connected','🔗');
    const px=cfg.prefix;
    for(let i=1;i<=APPS.length;i++)mqttClient.subscribe(`${px}/relay/${i}/state`);
    mqttClient.subscribe(`${px}/device/heartbeat`);
    mqttClient.subscribe(`${px}/device/status`);
    mqttClient.subscribe(`${px}/tank/level`);
    mqttClient.publish(`${px}/web/status`,'online',{retain:false});
  });
  mqttClient.on('message',(topic,msg)=>{
    const raw=msg.toString().trim(),val=raw.toUpperCase(),px=cfg.prefix;
    if(topic===`${px}/device/heartbeat`){lastHbTime=Date.now();updateESPDot();return;}
    if(topic===`${px}/device/status`){if(val==='OFFLINE'){lastHbTime=0;updateESPDot();setAllDevicesOffline();toast('ESP32 went offline','⚠');}return;}
    if(topic===`${px}/tank/level`){const n=parseFloat(raw);if(!isNaN(n)){tankLevel=n;updateTankGauge();}return;}
    for(let i=1;i<=APPS.length;i++){
      if(topic===`${px}/relay/${i}/state`){
        const on=val==='ON'||val==='1';
        if(relayState[i-1]!==on){relayState[i-1]=on;updateCard(i-1);}
        if(!deviceOnline[i-1]){deviceOnline[i-1]=true;updateConnDot(i-1);updateConnCount();}
        return;
      }
    }
  });
  mqttClient.on('reconnect',()=>{setMQTT('connecting');lastHbTime=0;updateESPDot();});
  mqttClient.on('offline',()=>{setMQTT('disconnected');lastHbTime=0;updateESPDot();setAllDevicesOffline();toast('MQTT disconnected','⚠');});
  mqttClient.on('error',e=>{console.warn(e);setMQTT('disconnected');});
}
function mqttPub(t,p){if(mqttClient&&mqttClient.connected)mqttClient.publish(t,String(p),{qos:1});else toast('MQTT not connected','⚠');}
function setMQTT(s){
  const d=document.getElementById('mqttDot'),l=document.getElementById('mqttLbl');
  d.className='dot';
  if(s==='connected'){d.classList.add('green');l.textContent='MQTT ●';}
  else if(s==='connecting'){d.classList.add('amber','pulse');l.textContent='MQTT…';}
  else{d.classList.add('red');l.textContent='MQTT ✕';}
}
function updateESPDot(){
  const age=lastHbTime===0?Infinity:(Date.now()-lastHbTime)/1000;
  const d=document.getElementById('espDot'),l=document.getElementById('espLbl');
  d.className='dot';
  if(age<40){d.classList.add('green');l.textContent='ESP32 ●';}
  else if(age<90){d.classList.add('amber');if(age>60)d.classList.add('pulse');l.textContent='ESP32 ~';}
  else{d.classList.add('red');l.textContent='ESP32 ✕';}
}

/* ── SPEED ── */
async function measureSpeed(){try{if(navigator.connection&&navigator.connection.downlink>0){document.getElementById('speedVal').textContent=(navigator.connection.downlink/8).toFixed(1);return;}const t=performance.now();const r=await fetch('https://www.cloudflare.com/cdn-cgi/trace?t='+Date.now(),{cache:'no-store'});const d=await r.text();const v=(new TextEncoder().encode(d).length/((performance.now()-t)/1000)/1024/1024).toFixed(1);document.getElementById('speedVal').textContent=v>100?'--':v;}catch{document.getElementById('speedVal').textContent='--';}}
function startSpeed(){measureSpeed();setInterval(measureSpeed,30000);if(navigator.connection)navigator.connection.addEventListener('change',measureSpeed);}

/* ── SETTINGS ── */
document.getElementById('settingsBtn').addEventListener('click',()=>{document.getElementById('cfgHost').value=cfg.host;document.getElementById('cfgPort').value=cfg.port;document.getElementById('cfgPrefix').value=cfg.prefix;document.getElementById('cfgUser').value=cfg.user;document.getElementById('cfgPass').value=cfg.pass;document.getElementById('settingsModal').classList.add('open');});
document.getElementById('mClose').addEventListener('click',()=>document.getElementById('settingsModal').classList.remove('open'));
document.getElementById('cfgSave').addEventListener('click',()=>{saveCfg({host:document.getElementById('cfgHost').value.trim()||'broker.emqx.io',port:document.getElementById('cfgPort').value.trim()||'8084',prefix:document.getElementById('cfgPrefix').value.trim()||'HOME_AUTO',user:document.getElementById('cfgUser').value.trim(),pass:document.getElementById('cfgPass').value.trim()});document.getElementById('settingsModal').classList.remove('open');connectMQTT();toast('Settings saved. Reconnecting…','⚙');});
document.getElementById('settingsModal').addEventListener('click',e=>{if(e.target===document.getElementById('settingsModal'))document.getElementById('settingsModal').classList.remove('open');});

/* ── LOGOUT ── */
document.getElementById('logoutBtn').addEventListener('click',()=>{sessionStorage.removeItem('hauth');if(mqttClient)mqttClient.end(true);if(hbTimer)clearInterval(hbTimer);lastHbTime=0;setAllDevicesOffline();document.getElementById('dashboard').style.display='none';document.getElementById('loginPage').style.display='flex';document.getElementById('pwInput').value='';});

/* ── TOAST ── */
function toast(msg,icon='ℹ'){const el=document.createElement('div');el.className='toast-msg';el.innerHTML=`<span>${icon}</span> ${msg}`;el.style.animationDuration='0.3s,0.3s';el.style.animationDelay='0s,2.2s';document.getElementById('toast').appendChild(el);setTimeout(()=>el.remove(),2700);}
</script>
</body>
</html>
