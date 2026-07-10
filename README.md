<9751640009>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width,initial-scale=1,viewport-fit=cover">
<title>THE HOME PANEL</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link href="https://fonts.googleapis.com/css2?family=Fraunces:wght@600;700&family=Nunito:wght@400;600;700;800&family=JetBrains+Mono:wght@400;700&display=swap" rel="stylesheet">
<script src="https://unpkg.com/mqtt/dist/mqtt.min.js"></script>
<style>
/* ── WARM DUSK / LIT-WINDOW HOME THEME ──────────────────────── */
:root{
  --bg:#211b17;            /* dusk-sky charcoal-brown */
  --bg2:#2a2219;
  --surface:#2f2721;       /* warm timber panel */
  --surface2:#3a3128;
  --gold:#e8a33d;          /* lamp-glow amber */
  --gold2:#f0b563;
  --gold3:#f6cd8c;
  --gold-dim:rgba(232,163,61,0.12);
  --gold-glow:rgba(232,163,61,0.35);
  --on:#4caf6b;
  --on-bg:rgba(76,175,107,0.15);
  --on-glow:rgba(76,175,107,0.35);
  --off:#c0392b;
  --off-bg:rgba(192,57,43,0.13);
  --off-glow:rgba(192,57,43,0.30);
  --warn:#e67e22;
  --warn-glow:rgba(230,126,34,0.35);
  --text:#f3e9da;          /* warm parchment */
  --text2:#b3a18a;         /* warm taupe */
  --border:rgba(232,163,61,0.16);
  --border-h:rgba(232,163,61,0.42);
  --shadow:rgba(0,0,0,0.35);
  --shadow-md:rgba(0,0,0,0.5);
}
*,*::before,*::after{box-sizing:border-box;margin:0;padding:0}
html{height:100%}
body{
  background:var(--bg);
  color:var(--text);
  font-family:'Nunito',sans-serif;
  height:100%;
  overflow:hidden;
  background-image:
    radial-gradient(ellipse 70% 40% at 50% 0%,rgba(232,163,61,0.10) 0%,transparent 70%),
    radial-gradient(circle at 1px 1px,rgba(232,163,61,0.07) 1px,transparent 0);
  background-size:cover,24px 24px;
}

::-webkit-scrollbar{width:5px}
::-webkit-scrollbar-track{background:var(--bg2)}
::-webkit-scrollbar-thumb{background:var(--gold2);border-radius:3px}

/* ══ LOGIN PAGE ══ */
#loginPage{
  position:fixed;inset:0;z-index:900;
  display:flex;flex-direction:column;align-items:center;justify-content:center;
  padding:20px;background:var(--bg);
  background-image:radial-gradient(ellipse 70% 60% at 50% 50%,rgba(232,163,61,0.10) 0%,transparent 70%);
}
.lp-wrap{width:100%;max-width:380px;animation:fadeUp .7s cubic-bezier(.16,1,.3,1) both;}
@keyframes fadeUp{from{opacity:0;transform:translateY(20px)}to{opacity:1;transform:translateY(0)}}

.lp-logo{
  display:block;width:88px;height:88px;border-radius:50%;
  border:2.5px solid var(--gold2);
  box-shadow:0 0 0 5px rgba(232,163,61,0.14),0 6px 28px var(--shadow-md);
  object-fit:cover;margin:0 auto 14px;background:var(--surface);
  animation:logoPulse 3s ease-in-out infinite;
}
@keyframes logoPulse{
  0%,100%{box-shadow:0 0 0 5px rgba(232,163,61,0.14),0 6px 28px var(--shadow-md)}
  50%    {box-shadow:0 0 0 8px rgba(232,163,61,0.24),0 8px 36px rgba(0,0,0,0.4)}
}

.lp-card{
  background:var(--surface);border:1px solid var(--border);border-radius:20px;
  padding:36px 32px 32px;box-shadow:0 4px 24px var(--shadow-md), 0 1px 0 rgba(255,255,255,0.03) inset;
}
.lp-title{font-family:'Fraunces',serif;font-size:20px;font-weight:700;color:var(--gold2);text-align:center;letter-spacing:.3px;line-height:1.3;margin-bottom:2px;}
.lp-sub{font-family:'Nunito',sans-serif;font-size:10px;font-weight:700;color:var(--text2);text-align:center;letter-spacing:4px;text-transform:uppercase;margin-bottom:24px;}
.lp-divider{display:flex;align-items:center;gap:10px;margin-bottom:20px;}
.lp-divider::before,.lp-divider::after{content:'';flex:1;height:1px;background:linear-gradient(to right,transparent,var(--gold2),transparent);}
.lp-divider span{color:var(--gold2);font-size:12px;}
.lp-field label{display:block;font-size:10px;font-weight:700;letter-spacing:3px;color:var(--gold2);text-transform:uppercase;margin-bottom:6px;}
.pw-wrap{position:relative;margin-bottom:16px;}
.pw-wrap input{
  width:100%;background:var(--bg);border:1.5px solid var(--border);border-radius:10px;
  padding:12px 42px 12px 14px;font-family:'Nunito',sans-serif;font-size:16px;font-weight:600;
  color:var(--text);outline:none;letter-spacing:1px;transition:border-color .2s,box-shadow .2s;
}
.pw-wrap input:focus{border-color:var(--gold2);box-shadow:0 0 0 3px var(--gold-dim);}
.pw-wrap input::placeholder{color:var(--text2);letter-spacing:0.5px;}
.eye-btn{position:absolute;right:11px;top:50%;transform:translateY(-50%);background:none;border:none;cursor:pointer;color:var(--text2);font-size:16px;padding:4px;transition:color .2s;}
.eye-btn:hover{color:var(--gold2)}
.lp-btn{
  width:100%;padding:13px;border:none;border-radius:10px;
  background:linear-gradient(135deg,var(--gold),var(--gold3),var(--gold));background-size:200%;
  font-family:'Fraunces',serif;font-size:14px;font-weight:700;letter-spacing:1px;text-transform:uppercase;color:#231a10;
  cursor:pointer;box-shadow:0 3px 14px var(--gold-glow);transition:background-position .4s,box-shadow .3s,transform .12s;
}
.lp-btn:hover{background-position:right;box-shadow:0 5px 22px var(--gold-glow);transform:translateY(-1px);}
.lp-btn:active{transform:translateY(0)}
.lp-err{margin-top:12px;padding:9px 14px;background:var(--off-bg);border:1px solid rgba(192,57,43,0.3);border-radius:8px;font-size:12px;color:#ff8a75;text-align:center;display:none;}
@keyframes shake{0%,100%{transform:translateX(0)}20%,60%{transform:translateX(-7px)}40%,80%{transform:translateX(7px)}}

/* ══ DASHBOARD ══ */
#dashboard{display:none;height:100dvh;height:100vh;flex-direction:column;overflow:hidden;}

.hdr{
  display:flex;align-items:center;gap:10px;padding:8px 14px;background:var(--surface);
  border-bottom:2px solid var(--gold-dim);box-shadow:0 2px 8px var(--shadow);flex-shrink:0;position:relative;z-index:10;
}
.hdr::after{content:'';position:absolute;bottom:-2px;left:0;right:0;height:2px;background:linear-gradient(to right,transparent,var(--gold2),transparent);opacity:.5;}
.hdr-logo{width:36px;height:36px;border-radius:50%;border:2px solid var(--gold2);object-fit:cover;box-shadow:0 2px 8px var(--shadow-md);flex-shrink:0;background:var(--surface);}
.hdr-text{flex:1;min-width:0;}
.hdr-title{font-family:'Fraunces',serif;font-size:14px;font-weight:700;color:var(--gold2);letter-spacing:.2px;white-space:nowrap;overflow:hidden;text-overflow:ellipsis;}
.hdr-sub{font-family:'Nunito',sans-serif;font-size:8px;font-weight:700;color:var(--text2);letter-spacing:3px;text-transform:uppercase;}
.hdr-chips{display:flex;align-items:center;gap:5px;flex-shrink:0;}
.chip{
  display:flex;align-items:center;gap:5px;border:1.5px solid var(--border);border-radius:20px;padding:4px 9px;
  font-family:'Nunito',sans-serif;font-size:11px;font-weight:700;background:var(--surface2);white-space:nowrap;
  position:relative;cursor:default;box-shadow:0 1px 3px var(--shadow);color:var(--text2);
}
.chip:hover .chip-tip{display:block}
.chip-tip{display:none;position:absolute;top:calc(100%+7px);right:0;background:var(--surface);border:1.5px solid var(--border-h);border-radius:10px;padding:10px 14px;width:210px;font-size:10px;color:var(--text2);z-index:400;line-height:2;box-shadow:0 4px 16px var(--shadow-md);}
.chip-tip b{color:var(--text);display:block;margin-bottom:4px;}
.chip-tip .tr{display:flex;gap:7px;align-items:center;}
.chip-tip .td{width:7px;height:7px;border-radius:50%;flex-shrink:0;}
.dot{width:7px;height:7px;border-radius:50%;flex-shrink:0;transition:background .4s,box-shadow .4s;}
.dot.grey  {background:#6b6055}
.dot.green {background:var(--on);box-shadow:0 0 6px var(--on-glow)}
.dot.amber {background:var(--warn);box-shadow:0 0 6px var(--warn-glow)}
.dot.red   {background:var(--off);box-shadow:0 0 6px var(--off-glow)}
.dot.pulse {animation:dp 1.2s ease-in-out infinite}
@keyframes dp{0%,100%{opacity:1}50%{opacity:.25}}
.spd{font-family:'JetBrains Mono',monospace;font-size:10px;color:var(--gold2);}
.hdr-btns{display:flex;gap:5px;flex-shrink:0;}
.icon-btn{background:var(--surface2);border:1.5px solid var(--border);border-radius:7px;padding:5px 8px;color:var(--text2);cursor:pointer;font-size:14px;transition:all .2s;box-shadow:0 1px 3px var(--shadow);}
.icon-btn:hover{border-color:var(--gold2);color:var(--gold2);background:var(--gold-dim)}
.logout-btn{background:var(--off-bg);border:1.5px solid rgba(192,57,43,0.3);border-radius:7px;padding:5px 10px;color:#ff8a75;cursor:pointer;font-size:11px;font-family:'Nunito',sans-serif;font-weight:700;letter-spacing:1px;transition:all .2s;}
.logout-btn:hover{background:rgba(192,57,43,0.22);border-color:var(--off)}

.ctrl{display:flex;gap:8px;padding:7px 12px;background:var(--surface2);border-bottom:1px solid var(--border);flex-shrink:0;}
.ctrl-btn{
  flex:1;display:flex;align-items:center;justify-content:center;gap:5px;padding:8px 4px;border-radius:9px;border:1.5px solid;
  font-family:'Nunito',sans-serif;font-size:12px;font-weight:800;letter-spacing:1px;text-transform:uppercase;cursor:pointer;
  transition:all .2s;box-shadow:0 1px 4px var(--shadow);
}
.ctrl-on{background:var(--on-bg);border-color:rgba(76,175,107,0.4);color:#7fd39a;}
.ctrl-on:hover{background:rgba(76,175,107,0.22);box-shadow:0 3px 12px var(--on-glow);}
.ctrl-off{background:var(--off-bg);border-color:rgba(192,57,43,0.35);color:#ff8a75;}
.ctrl-off:hover{background:rgba(192,57,43,0.22);box-shadow:0 3px 12px var(--off-glow);}
.ctrl-sch{background:var(--gold-dim);border-color:var(--border);color:var(--gold2);}
.ctrl-sch:hover{background:rgba(232,163,61,0.20);border-color:var(--gold2);}

/* ── 2-column scrollable "window" grid for 14 devices ── */
.grid{
  flex:1;display:grid;grid-template-columns:1fr 1fr;grid-auto-rows:minmax(98px,1fr);
  gap:6px;padding:7px;overflow-y:auto;min-height:0;background:var(--bg);
}

.card{
  background:var(--surface);border:1.5px solid var(--border);border-radius:12px 12px 8px 8px;
  display:flex;flex-direction:column;align-items:center;justify-content:space-between;
  padding:6px 7px 7px;cursor:pointer;transition:border-color .25s,box-shadow .25s,transform .12s,background .3s;
  position:relative;overflow:hidden;min-height:0;box-shadow:0 2px 6px var(--shadow);
  -webkit-tap-highlight-color:transparent;user-select:none;
}
.card::before{content:'';position:absolute;top:0;left:0;right:0;height:3px;background:linear-gradient(to right,transparent,var(--gold3),transparent);opacity:.5;transition:opacity .25s;}
.card:hover{border-color:var(--border-h);transform:translateY(-1px);box-shadow:0 5px 16px var(--shadow-md);}
.card:hover::before{opacity:1}
.card:active{transform:scale(0.97);}

/* lit-window glow when device is on */
.card.is-on{
  border-color:rgba(76,175,107,0.45);
  background:linear-gradient(160deg,rgba(76,175,107,0.09),rgba(232,163,61,0.06));
  box-shadow:0 2px 6px var(--shadow),0 0 0 1px rgba(76,175,107,0.22),0 0 18px rgba(232,163,61,0.10);
}
.card.is-on::before{background:linear-gradient(to right,transparent,var(--on),transparent);opacity:.5;}

.card-icon{font-size:19px;line-height:1;flex-shrink:0;}
.card-name{
  font-family:'Nunito',sans-serif;font-size:11px;font-weight:800;color:var(--text);letter-spacing:.4px;
  text-transform:uppercase;text-align:center;line-height:1.15;word-break:break-word;
}
.card-num{position:absolute;top:4px;right:5px;font-family:'JetBrains Mono',monospace;font-size:8px;color:var(--text2);border:1px solid var(--border);border-radius:3px;padding:1px 4px;background:var(--surface2);}

.card-status{display:flex;flex-direction:column;align-items:center;gap:3px;flex-shrink:0;}
.orb{width:30px;height:30px;border-radius:50%;display:flex;align-items:center;justify-content:center;transition:all .4s;border:2px solid transparent;}
.orb.on{background:var(--on-bg);border-color:rgba(76,175,107,0.4);box-shadow:0 0 10px var(--on-glow);}
.orb.off{background:var(--off-bg);border-color:rgba(192,57,43,0.3);}
.orb-dot{width:13px;height:13px;border-radius:50%;transition:all .4s;}
.on .orb-dot{background:var(--on);box-shadow:0 0 7px var(--on);}
.off .orb-dot{background:var(--off);}
.state-lbl{font-family:'JetBrains Mono',monospace;font-size:8px;font-weight:700;letter-spacing:1.5px;transition:color .3s;}
.state-lbl.on{color:#7fd39a}.state-lbl.off{color:#ff8a75}

.tog{
  width:100%;padding:5px 4px;border-radius:7px;border:1.5px solid;font-family:'Nunito',sans-serif;font-size:10px;
  font-weight:800;letter-spacing:.6px;text-transform:uppercase;cursor:pointer;transition:all .2s;flex-shrink:0;box-shadow:0 1px 3px var(--shadow);
}
.tog.off{background:var(--on-bg);border-color:rgba(76,175,107,0.4);color:#7fd39a;}
.tog.on {background:var(--off-bg);border-color:rgba(192,57,43,0.35);color:#ff8a75;}
.tog.off:hover{background:rgba(76,175,107,0.22);}
.tog.on:hover {background:rgba(192,57,43,0.22);}

/* ══ SCHEDULE PANEL ══ */
.panel-overlay{display:none;position:fixed;inset:0;z-index:500;background:rgba(10,7,5,.55);backdrop-filter:blur(4px);}
.panel-overlay.open{display:block}
.panel{
  position:fixed;bottom:0;left:0;right:0;z-index:501;background:var(--surface);border-top:2px solid var(--gold2);
  border-radius:18px 18px 0 0;padding:0 0 env(safe-area-inset-bottom,0);max-height:90vh;
  transform:translateY(100%);transition:transform .35s cubic-bezier(.16,1,.3,1);display:flex;flex-direction:column;
  box-shadow:0 -4px 24px var(--shadow-md);
}
.panel-overlay.open .panel{transform:translateY(0)}
.panel-drag{width:40px;height:4px;border-radius:2px;background:var(--border);margin:10px auto 0;}
.panel-head{display:flex;align-items:center;justify-content:space-between;padding:12px 16px 10px;border-bottom:1px solid var(--border);flex-shrink:0;}
.panel-title{font-family:'Fraunces',serif;font-size:15px;font-weight:700;color:var(--gold2);letter-spacing:.3px;}
.panel-close{background:var(--off-bg);border:1.5px solid rgba(192,57,43,0.3);border-radius:6px;padding:4px 9px;color:#ff8a75;cursor:pointer;font-size:13px;transition:all .2s;}
.panel-close:hover{background:rgba(192,57,43,0.22);}
.panel-body{overflow-y:auto;flex:1;padding:12px 14px 16px;}

.sch-block{margin-bottom:14px;background:var(--surface2);border:1.5px solid var(--border);border-radius:10px;overflow:hidden;box-shadow:0 1px 4px var(--shadow);}
.sch-block-head{display:flex;align-items:center;gap:8px;padding:8px 12px;background:var(--bg2);border-bottom:1px solid var(--border);}
.sch-block-icon{font-size:16px}
.sch-block-name{font-family:'Nunito',sans-serif;font-size:13px;font-weight:800;color:var(--text);letter-spacing:.4px;flex:1;}
.sch-block-body{padding:8px 10px;}
.sch-entries{display:flex;flex-direction:column;gap:4px;margin-bottom:8px;}
.sch-entry{display:flex;align-items:center;gap:6px;background:var(--surface);border:1px solid var(--border);border-radius:6px;padding:5px 8px;}
.sdot{width:6px;height:6px;border-radius:50%;flex-shrink:0;}
.sdot.on{background:var(--on);}.sdot.off{background:var(--off);}
.se-txt{flex:1;font-family:'Nunito',sans-serif;font-size:11px;color:var(--text);}
.se-day{color:var(--gold2);font-size:10px;font-weight:800;}.se-time{color:var(--text);font-family:'JetBrains Mono',monospace;font-size:10px;}.se-act{font-size:10px;font-weight:800;}
.se-act.on{color:#7fd39a}.se-act.off{color:#ff8a75}
.se-del{background:var(--off-bg);border:1px solid rgba(192,57,43,0.28);border-radius:4px;width:18px;height:18px;display:flex;align-items:center;justify-content:center;cursor:pointer;color:#ff8a75;font-size:10px;flex-shrink:0;transition:all .2s;}
.se-del:hover{background:rgba(192,57,43,0.3);}
.sch-none{font-size:11px;color:var(--text2);font-style:italic;padding:2px 0 6px;}
.sch-add-lbl{font-size:10px;font-weight:800;letter-spacing:2px;color:var(--text2);text-transform:uppercase;margin-bottom:6px;}
.sch-form{display:grid;grid-template-columns:1fr 1fr;gap:5px;}
.sch-form select,.sch-form input[type=time],.sch-form .sch-btn{
  background:var(--surface);border:1.5px solid var(--border);border-radius:6px;padding:6px 8px;
  font-family:'Nunito',sans-serif;font-size:12px;font-weight:600;color:var(--text);outline:none;-webkit-appearance:none;appearance:none;transition:border-color .2s;
}
.sch-form select:focus,.sch-form input[type=time]:focus{border-color:var(--gold2);}
.sch-form select option{background:var(--surface);color:var(--text);}
input[type=time]::-webkit-calendar-picker-indicator{opacity:.6;cursor:pointer;filter:invert(1);}
.sch-btn{grid-column:span 2;background:var(--gold-dim);border-color:var(--gold2);color:var(--gold2);font-weight:800;letter-spacing:1px;text-transform:uppercase;cursor:pointer;transition:all .2s;text-align:center;}
.sch-btn:hover{background:rgba(232,163,61,0.22);}

/* ══ SETTINGS MODAL ══ */
.modal-overlay{display:none;position:fixed;inset:0;z-index:600;background:rgba(10,7,5,.55);backdrop-filter:blur(6px);align-items:flex-end;justify-content:center;}
.modal-overlay.open{display:flex}
.modal{background:var(--surface);border:1.5px solid var(--gold2);border-radius:18px 18px 0 0;padding:20px 18px calc(env(safe-area-inset-bottom,0)+20px);width:100%;max-width:460px;animation:slideUp .3s cubic-bezier(.16,1,.3,1) both;box-shadow:0 -4px 24px var(--shadow-md);}
@keyframes slideUp{from{transform:translateY(30px);opacity:0}to{transform:translateY(0);opacity:1}}
.modal-title{font-family:'Fraunces',serif;font-size:16px;font-weight:700;color:var(--gold2);letter-spacing:.3px;margin-bottom:18px;display:flex;align-items:center;justify-content:space-between;}
.modal-close{background:var(--off-bg);border:1.5px solid rgba(192,57,43,0.3);border-radius:6px;padding:3px 9px;color:#ff8a75;cursor:pointer;font-size:13px;}
.mf{margin-bottom:12px}
.mf label{display:block;font-size:10px;font-weight:800;letter-spacing:2px;color:var(--gold2);text-transform:uppercase;margin-bottom:5px;}
.mf input{width:100%;background:var(--bg);border:1.5px solid var(--border);border-radius:8px;padding:9px 12px;font-family:'Nunito',sans-serif;font-size:13px;font-weight:600;color:var(--text);outline:none;transition:border-color .2s;}
.mf input:focus{border-color:var(--gold2)}
.mf .hint{font-size:10px;color:var(--text2);margin-top:4px;font-style:italic;}
.modal-save{width:100%;margin-top:14px;padding:12px;background:linear-gradient(135deg,var(--gold),var(--gold3));border:none;border-radius:9px;font-family:'Fraunces',serif;font-size:14px;font-weight:700;letter-spacing:.5px;color:#231a10;cursor:pointer;box-shadow:0 3px 12px var(--gold-glow);transition:opacity .2s,transform .12s;}
.modal-save:hover{opacity:.88;transform:translateY(-1px);}

/* ══ TOAST ══ */
#toast{position:fixed;bottom:20px;left:50%;transform:translateX(-50%);z-index:999;display:flex;flex-direction:column;align-items:center;gap:6px;pointer-events:none;width:calc(100% - 32px);max-width:340px;}
.toast-msg{background:var(--surface);border:1.5px solid var(--border);border-radius:10px;padding:10px 16px;font-family:'Nunito',sans-serif;font-size:13px;font-weight:700;color:var(--text);box-shadow:0 4px 16px var(--shadow-md);animation:tIn .3s cubic-bezier(.16,1,.3,1) both,tOut .3s ease .22s forwards;display:flex;align-items:center;gap:7px;width:100%;}
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
      <div class="lp-title">THE HOME<br>PANEL</div>
      <div class="lp-sub">Secure Access Portal</div>
      <div class="lp-divider"><span>✦</span></div>
      <div class="lp-field">
        <label>Access Password</label>
        <div class="pw-wrap">
          <input type="password" id="pwInput" placeholder="Enter password…" autocomplete="current-password">
          <button class="eye-btn" id="eyeBtn" tabindex="-1">👁</button>
        </div>
      </div>
      <button class="lp-btn" id="loginBtn">🔑 ENTER HOME</button>
      <div class="lp-err" id="loginErr">⛔ &nbsp;Incorrect password. Try again.</div>
    </div>
  </div>
</div>

<!-- ══ DASHBOARD ══ -->
<div id="dashboard">
  <div class="hdr">
    <img class="hdr-logo" id="hdrLogo" src="" alt="">
    <div class="hdr-text">
      <div class="hdr-title">THE HOME PANEL</div>
      <div class="hdr-sub">Room by Room</div>
    </div>
    <div class="hdr-chips">
      <div class="chip">
        <span>📶</span><span class="spd" id="speedVal">--</span><span style="font-size:9px;color:var(--text2)">M/s</span>
      </div>
      <div class="chip" id="espChip">
        <div class="dot grey pulse" id="espDot"></div>
        <span id="espLbl">ESP32</span>
        <div class="chip-tip">
          <b>ESP32 Live Status</b>
          <div class="tr"><span class="td" style="background:var(--on)"></span>Heartbeat &lt;40s ago</div>
          <div class="tr"><span class="td" style="background:var(--warn)"></span>No heartbeat 40–90s</div>
          <div class="tr"><span class="td" style="background:var(--off)"></span>Offline / &gt;90s</div>
        </div>
      </div>
      <div class="chip">
        <div class="dot grey" id="mqttDot"></div>
        <span id="mqttLbl">MQTT</span>
      </div>
    </div>
    <div class="hdr-btns">
      <button class="icon-btn" id="settingsBtn" title="Settings">⚙</button>
      <button class="logout-btn" id="logoutBtn">OUT</button>
    </div>
  </div>

  <div class="ctrl">
    <button class="ctrl-btn ctrl-on"  onclick="allOn()">✅ ALL ON</button>
    <button class="ctrl-btn ctrl-off" onclick="allOff()">❌ ALL OFF</button>
    <button class="ctrl-btn ctrl-sch" onclick="openSchedules()">⏰ SCHEDULES</button>
  </div>

  <div class="grid" id="appGrid"></div>
</div>

<!-- ══ SCHEDULE PANEL ══ -->
<div class="panel-overlay" id="panelOverlay">
  <div class="panel" id="schedPanel">
    <div class="panel-drag"></div>
    <div class="panel-head">
      <span class="panel-title">⏰ SCHEDULES</span>
      <button class="panel-close" onclick="closeSchedules()">✕ CLOSE</button>
    </div>
    <div class="panel-body" id="panelBody"></div>
  </div>
</div>

<!-- ══ SETTINGS MODAL ══ -->
<div class="modal-overlay" id="settingsModal">
  <div class="modal">
    <div class="modal-title">
      ⚙ MQTT Settings
      <button class="modal-close" id="mClose">✕</button>
    </div>
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
/* ── LOGO: little house with one lit, one dark window ── */
const LOGO_SRC='data:image/svg+xml;utf8,'+encodeURIComponent(`
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 100 100">
  <rect width="100" height="100" fill="#2f2721"/>
  <path d="M50 18 L84 46 H76 V82 H24 V46 H16 Z" fill="none" stroke="#e8a33d" stroke-width="5" stroke-linejoin="round"/>
  <rect x="33" y="53" width="15" height="15" fill="#f6cd8c"/>
  <rect x="52" y="53" width="15" height="15" fill="#221b16" stroke="#e8a33d" stroke-width="2"/>
  <rect x="43" y="70" width="14" height="12" fill="#e8a33d"/>
</svg>`);
document.getElementById('loginLogo').src=LOGO_SRC;
document.getElementById('hdrLogo').src=LOGO_SRC;

/* ── APPLIANCES (14 home devices) ── */
const APPS=[
  {id:1,  name:'LIVING ROOM LT',  icon:'💡'},
  {id:2,  name:'BEDROOM LIGHT',   icon:'🛏'},
  {id:3,  name:'KITCHEN LIGHT',   icon:'🍳'},
  {id:4,  name:'HALL LIGHT',      icon:'🔆'},
  {id:5,  name:'PORCH LIGHT',     icon:'🏮'},
  {id:6,  name:'BALCONY LIGHT',   icon:'🪟'},
  {id:7,  name:'CEILING FAN 1',   icon:'🌀'},
  {id:8,  name:'CEILING FAN 2',   icon:'🌀'},
  {id:9,  name:'TV / SET-TOP',    icon:'📺'},
  {id:10, name:'WATER HEATER',    icon:'🔥'},
  {id:11, name:'WATER PUMP',      icon:'🚰'},
  {id:12, name:'GARAGE DOOR',     icon:'🚪'},
  {id:13, name:'WASHING MACHINE', icon:'🧺'},
  {id:14, name:'GARDEN SPRINKLER',icon:'🌿'},
];
const DAY_OPTS=[
  {v:'DAILY',l:'Every Day'},{v:'SUN',l:'Sun'},{v:'MON',l:'Mon'},
  {v:'TUE',l:'Tue'},{v:'WED',l:'Wed'},{v:'THU',l:'Thu'},
  {v:'FRI',l:'Fri'},{v:'SAT',l:'Sat'},
];

/* ── STATE ── */
let relayState=Array(APPS.length).fill(false);
let schedules=JSON.parse(localStorage.getItem('home_sch_v1')||'{}');
let mqttClient=null;
let cfg=loadCfg();
let lastHbTime=0;
let hbTimer=null;

function loadCfg(){
  return{host:localStorage.getItem('mq_host')||'broker.emqx.io',port:localStorage.getItem('mq_port')||'8084',prefix:localStorage.getItem('mq_pfx')||'HOME_AUTO',user:localStorage.getItem('mq_user')||'',pass:localStorage.getItem('mq_pass')||''};
}
function saveCfg(c){localStorage.setItem('mq_host',c.host);localStorage.setItem('mq_port',c.port);localStorage.setItem('mq_pfx',c.prefix);localStorage.setItem('mq_user',c.user);localStorage.setItem('mq_pass',c.pass);cfg=c;}
function saveSch(){localStorage.setItem('home_sch_v1',JSON.stringify(schedules));}

/* ── LOGIN (default password: home123 — change PWD below) ── */
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
  buildGrid();connectMQTT();startSpeed();
  if(hbTimer)clearInterval(hbTimer);
  hbTimer=setInterval(updateESPDot,5000);
  updateESPDot();
}

/* ── GRID ── */
function buildGrid(){
  const g=document.getElementById('appGrid');g.innerHTML='';
  APPS.forEach(a=>{
    const s=relayState[a.id-1];
    const d=document.createElement('div');
    d.className='card'+(s?' is-on':'');d.id='card'+a.id;
    d.innerHTML=`
      <span class="card-num">${a.id}</span>
      <span class="card-icon">${a.icon}</span>
      <span class="card-name">${a.name}</span>
      <div class="card-status">
        <div class="orb ${s?'on':'off'}" id="orb${a.id}"><div class="orb-dot"></div></div>
        <span class="state-lbl ${s?'on':'off'}" id="lbl${a.id}">${s?'ON':'OFF'}</span>
      </div>
      <button class="tog ${s?'on':'off'}" id="tog${a.id}">${s?'⏹ OFF':'▶ ON'}</button>`;
    d.querySelector('.tog').addEventListener('click',e=>{e.stopPropagation();toggle(a.id);});
    d.addEventListener('click',()=>toggle(a.id));
    g.appendChild(d);
  });
}
function updateCard(idx){
  const a=APPS[idx],s=relayState[idx];
  const card=document.getElementById('card'+a.id);if(!card)return;
  card.className='card'+(s?' is-on':'');
  document.getElementById('orb'+a.id).className='orb '+(s?'on':'off');
  document.getElementById('lbl'+a.id).className='state-lbl '+(s?'on':'off');
  document.getElementById('lbl'+a.id).textContent=s?'ON':'OFF';
  document.getElementById('tog'+a.id).className='tog '+(s?'on':'off');
  document.getElementById('tog'+a.id).textContent=s?'⏹ OFF':'▶ ON';
}
function updateAllCards(){relayState.forEach((_,i)=>updateCard(i));}

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
        <span class="sch-block-icon">${a.icon}</span>
        <span class="sch-block-name">${a.name}</span>
        <span style="font-size:10px;color:var(--text2)">${list.length} set</span>
      </div>
      <div class="sch-block-body">
        <div class="sch-entries">${entries}</div>
        <div class="sch-add-lbl">➕ Add Schedule</div>
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
  lastHbTime=0;updateESPDot();setMQTT('connecting');
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
    mqttClient.publish(`${px}/web/status`,'online',{retain:false});
  });
  mqttClient.on('message',(topic,msg)=>{
    const val=msg.toString().trim().toUpperCase(),px=cfg.prefix;
    if(topic===`${px}/device/heartbeat`){lastHbTime=Date.now();updateESPDot();return;}
    if(topic===`${px}/device/status`){if(val==='OFFLINE'){lastHbTime=0;updateESPDot();toast('ESP32 went offline','⚠');}return;}
    for(let i=1;i<=APPS.length;i++){
      if(topic===`${px}/relay/${i}/state`){const on=val==='ON'||val==='1';if(relayState[i-1]!==on){relayState[i-1]=on;updateCard(i-1);}return;}
    }
  });
  mqttClient.on('reconnect',()=>{setMQTT('connecting');lastHbTime=0;updateESPDot();});
  mqttClient.on('offline',()=>{setMQTT('disconnected');lastHbTime=0;updateESPDot();toast('MQTT disconnected','⚠');});
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
document.getElementById('logoutBtn').addEventListener('click',()=>{sessionStorage.removeItem('hauth');if(mqttClient)mqttClient.end(true);if(hbTimer)clearInterval(hbTimer);lastHbTime=0;document.getElementById('dashboard').style.display='none';document.getElementById('loginPage').style.display='flex';document.getElementById('pwInput').value='';});

/* ── TOAST ── */
function toast(msg,icon='ℹ'){const el=document.createElement('div');el.className='toast-msg';el.innerHTML=`<span>${icon}</span> ${msg}`;el.style.animationDuration='0.3s,0.3s';el.style.animationDelay='0s,2.2s';document.getElementById('toast').appendChild(el);setTimeout(()=>el.remove(),2700);}
</script>
</body>
</html>
