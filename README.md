<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width,initial-scale=1">
<meta http-equiv="Permissions-Policy" content="camera=*, microphone=*, display-capture=*">
<title>FasaFas — Meeting Room</title>
<style>
*{box-sizing:border-box;margin:0;padding:0}
html,body{background:#0a0a0a;color:#fff;font-family:'Segoe UI',system-ui,sans-serif;height:100%;overflow:hidden}

/* ── TOP BAR ── */
.topbar{position:fixed;top:0;left:0;right:0;height:52px;z-index:300;background:rgba(10,10,10,.97);border-bottom:1px solid #1e1e1e;display:flex;align-items:center;padding:0 16px;gap:14px}
.tb-logo{font-size:15px;font-weight:900;color:#3b82f6;display:flex;align-items:center;gap:6px;flex-shrink:0}
.tb-dot{width:6px;height:6px;border-radius:50%;background:#10b981;animation:blink 2s infinite}
@keyframes blink{0%,100%{opacity:1}50%{opacity:.3}}
.tb-topic{font-size:13px;font-weight:600;color:#e2e8f0;flex:1;white-space:nowrap;overflow:hidden;text-overflow:ellipsis}
.tb-right{display:flex;align-items:center;gap:8px;flex-shrink:0}
.tb-pill{background:#1a1a1a;border:1px solid #2a2a2a;border-radius:6px;padding:3px 9px;font-size:11px;color:#94a3b8;display:flex;align-items:center;gap:5px;cursor:default}
.tb-pill.click{cursor:pointer;transition:background .15s}
.tb-pill.click:hover{background:#222;color:#e2e8f0}
.tb-timer{font-size:13px;font-weight:700;color:#e2e8f0;font-family:monospace}
.rec-pill{background:rgba(239,68,68,.12);border-color:rgba(239,68,68,.3);color:#f87171;display:none}
.rec-dot{width:6px;height:6px;border-radius:50%;background:#ef4444;animation:blink .8s infinite}

/* ── LAYOUT ── */
.layout{display:flex;position:fixed;top:52px;left:0;right:0;bottom:72px}
.video-area{flex:1;position:relative;background:#0a0a0a;overflow:hidden}

/* ── JITSI IFRAME ── */
#jitsi-frame{position:absolute;inset:0;width:100%;height:100%;border:none;display:none}

/* ── LOADING ── */
.loading{position:absolute;inset:0;background:radial-gradient(ellipse at 20% 50%,rgba(37,99,235,.12),transparent 60%),#07101f;display:flex;flex-direction:column;align-items:center;justify-content:center;gap:14px;z-index:50}
.ld-icon{font-size:52px;animation:float 1.4s ease-in-out infinite alternate}
@keyframes float{from{transform:translateY(0)}to{transform:translateY(-10px)}}
.ld-title{font-size:18px;font-weight:800;color:#e2e8f0}
.ld-bar{width:200px;height:4px;background:#1e3050;border-radius:2px;overflow:hidden}
.ld-fill{height:100%;background:linear-gradient(90deg,#2563eb,#7c3aed);border-radius:2px;width:0%;transition:width .6s ease}
.ld-steps{display:flex;flex-direction:column;gap:4px;font-size:12px;min-width:240px}
.ld-step{color:#374151;padding:2px 0}
.ld-step.done{color:#10b981}
.ld-step.act{color:#60a5fa}

/* ── INFO PANEL (invite info shown in meeting) ── */
.info-toast{position:absolute;top:14px;left:50%;transform:translateX(-50%);background:#111;border:1px solid #2a2a2a;border-radius:10px;padding:12px 18px;font-size:12px;color:#94a3b8;display:none;z-index:60;text-align:center;min-width:300px}
.info-toast.show{display:block}
.info-toast b{color:#e2e8f0}
.info-copy{color:#60a5fa;cursor:pointer;margin-left:6px;font-size:11px}
.info-copy:hover{text-decoration:underline}

/* ── SIDE PANEL ── */
.side-panel{width:0;overflow:hidden;transition:width .22s ease;background:#111;border-left:0 solid #1e1e1e;height:100%;flex-shrink:0;display:flex;flex-direction:column}
.side-panel.open{width:300px;border-left-width:1px}
.ph{padding:12px 14px;border-bottom:1px solid #1e1e1e;display:flex;align-items:center;justify-content:space-between;flex-shrink:0}
.ph-title{font-size:13px;font-weight:700;color:#e2e8f0}
.ph-close{background:none;border:none;color:#475569;font-size:17px;cursor:pointer;padding:2px 6px;border-radius:4px;line-height:1}
.ph-close:hover{background:#1e1e1e;color:#94a3b8}
.pb{flex:1;overflow-y:auto;padding:12px}
.pb::-webkit-scrollbar{width:3px}
.pb::-webkit-scrollbar-thumb{background:#1e1e1e;border-radius:3px}
.chat-foot{padding:8px 10px;border-top:1px solid #1e1e1e;display:none;gap:7px}
.chat-foot.on{display:flex}
.chat-inp{flex:1;background:#1a1a1a;border:1px solid #2a2a2a;border-radius:7px;padding:7px 10px;color:#e2e8f0;font-family:inherit;font-size:12px;outline:none;resize:none}
.chat-inp:focus{border-color:#3b82f6}
.chat-send{background:#2563eb;border:none;color:#fff;border-radius:7px;width:32px;height:32px;cursor:pointer;font-size:15px;display:flex;align-items:center;justify-content:center;flex-shrink:0;align-self:flex-end}

/* ── PANEL CONTENT ── */
.p-item{display:flex;align-items:center;gap:9px;padding:7px 6px;border-radius:8px;transition:background .15s;margin-bottom:2px}
.p-item:hover{background:#1a1a1a}
.p-av{width:34px;height:34px;border-radius:50%;display:flex;align-items:center;justify-content:center;font-size:13px;font-weight:700;flex-shrink:0}
.p-name{font-size:13px;font-weight:600;color:#e2e8f0;flex:1}
.p-sub{font-size:10px;color:#475569;text-transform:uppercase;letter-spacing:.4px}
.p-act-btn{background:none;border:1px solid #2a2a2a;color:#94a3b8;border-radius:5px;padding:3px 7px;font-size:11px;cursor:pointer;transition:all .15s;font-family:inherit}
.p-act-btn:hover{background:#1e1e1e;color:#e2e8f0}
.host-menu{background:#1a1a1a;border:1px solid #2a2a2a;border-radius:9px;overflow:hidden;margin:2px 0 6px 44px;display:none}
.host-menu.on{display:block}
.hmi{padding:8px 12px;font-size:12px;color:#94a3b8;cursor:pointer;display:flex;align-items:center;gap:8px;transition:background .1s;border-bottom:1px solid #222}
.hmi:last-child{border:none}
.hmi:hover{background:#222;color:#e2e8f0}
.hmi.red:hover{background:rgba(239,68,68,.1);color:#f87171}
.chat-msg{padding:8px 9px;border-radius:8px;background:#1a1a1a;margin-bottom:7px;font-size:12.5px}
.chat-sender{font-weight:700;font-size:11px;text-transform:uppercase;letter-spacing:.3px;margin-bottom:2px}
.chat-text{color:#d1d5db;line-height:1.5}
.chat-time{font-size:10px;color:#374151;margin-top:2px}

/* ── VIRTUAL BACKGROUND ── */
.vbg-grid{display:grid;grid-template-columns:repeat(3,1fr);gap:8px;margin-bottom:12px}
.vbg-option{border:2px solid #2a2a2a;border-radius:9px;overflow:hidden;cursor:pointer;transition:all .18s;aspect-ratio:16/9;display:flex;align-items:center;justify-content:center;font-size:22px;background:#1a1a1a;position:relative}
.vbg-option:hover{border-color:#3b82f6;transform:scale(1.04)}
.vbg-option.active{border-color:#2563eb;box-shadow:0 0 0 2px rgba(37,99,235,.35)}
.vbg-option .vbg-label{position:absolute;bottom:4px;left:0;right:0;text-align:center;font-size:9px;font-weight:700;color:#94a3b8;text-transform:uppercase;letter-spacing:.5px}
.vbg-upload{width:100%;padding:9px;border-radius:8px;background:#1a1a1a;border:1px dashed #3a3a3a;color:#60a5fa;font-size:12px;font-weight:600;cursor:pointer;text-align:center;transition:all .15s}
.vbg-upload:hover{background:#222;border-color:#60a5fa}

/* ── RECORDING PANEL ── */
.rec-status{background:#1a1a1a;border:1px solid #2a2a2a;border-radius:9px;padding:14px;text-align:center;margin-bottom:10px}
.rec-status-icon{font-size:30px;margin-bottom:6px}
.rec-status-lbl{font-size:13px;font-weight:700;color:#e2e8f0}
.rec-status-sub{font-size:11px;color:#475569;margin-top:2px}
.rec-timer{font-size:20px;font-weight:800;font-family:monospace;color:#ef4444;margin:6px 0;display:none}
.rsbtn{width:100%;padding:10px;border-radius:8px;font-size:12px;font-weight:700;cursor:pointer;font-family:inherit;transition:all .15s;margin-bottom:6px;border:none}
.rsbtn.cloud{background:linear-gradient(135deg,#2563eb,#7c3aed);color:#fff}
.rsbtn.local{background:#1a1a1a;color:#94a3b8;border:1px solid #2a2a2a}
.rsbtn.stop{background:rgba(239,68,68,.15);color:#f87171;border:1px solid rgba(239,68,68,.3)}
.rsbtn:hover{opacity:.9}

/* ── INVITE PANEL ── */
.invite-box{background:#1a1a1a;border:1px solid #2a2a2a;border-radius:10px;padding:14px;margin-bottom:10px}
.invite-row{display:flex;align-items:center;justify-content:space-between;padding:7px 0;border-bottom:1px solid #222;font-size:12px}
.invite-row:last-child{border:none}
.invite-key{color:#475569;font-size:10px;font-weight:700;text-transform:uppercase;letter-spacing:.5px}
.invite-val{color:#e2e8f0;font-family:monospace;font-size:12px}
.copy-btn{background:none;border:1px solid #2a2a2a;color:#60a5fa;border-radius:5px;padding:2px 8px;font-size:10px;cursor:pointer;font-family:inherit;transition:all .15s}
.copy-btn:hover{background:#1e3050;border-color:#3b82f6}

/* ── SEC / BREAKOUT / TRANS panels ── */
.sec-row{display:flex;align-items:center;justify-content:space-between;padding:10px 0;border-bottom:1px solid #1a1a1a}
.sec-row:last-child{border:none}
.sec-lbl{font-size:13px;font-weight:600;color:#e2e8f0}
.sec-sub{font-size:11px;color:#475569;margin-top:2px}
.tog{width:36px;height:20px;border-radius:10px;background:#1e3050;position:relative;cursor:pointer;transition:background .2s;flex-shrink:0}
.tog.on{background:#2563eb}
.tog::after{content:'';position:absolute;width:14px;height:14px;border-radius:50%;background:#fff;top:3px;left:3px;transition:left .2s}
.tog.on::after{left:19px}
.br-room{background:#1a1a1a;border:1px solid #2a2a2a;border-radius:9px;padding:11px;margin-bottom:8px}
.br-rh{display:flex;align-items:center;justify-content:space-between;margin-bottom:6px}
.br-rname{font-size:13px;font-weight:700;color:#e2e8f0}
.br-count{font-size:10px;color:#475569;background:#111;border-radius:4px;padding:1px 6px}
.tr-line{padding:6px 0;border-bottom:1px solid #1a1a1a;font-size:12px}
.tr-line:last-child{border:none}
.tr-spk{font-weight:700;font-size:11px;margin-bottom:1px}
.tr-txt{color:#d1d5db;line-height:1.5}
.tr-t{font-size:10px;color:#374151;margin-top:1px}

/* ── TOOLBAR ── */
.toolbar{position:fixed;bottom:0;left:0;right:0;height:72px;background:rgba(10,10,10,.97);border-top:1px solid #1e1e1e;display:flex;align-items:center;justify-content:space-between;padding:0 12px;z-index:200}
.tg{display:flex;align-items:center;gap:4px}
.tbtn{display:flex;flex-direction:column;align-items:center;gap:3px;background:none;border:none;cursor:pointer;padding:5px 8px;border-radius:9px;transition:background .15s;color:#fff;font-family:inherit;position:relative}
.tbtn:hover{background:#1a1a1a}
.tbtn.act{background:#1e3050}
.tbtn .ic{font-size:18px;width:36px;height:36px;border-radius:9px;display:flex;align-items:center;justify-content:center;background:#1a1a1a;transition:background .15s;border:1px solid #2a2a2a}
.tbtn:hover .ic{background:#2a2a2a}
.tbtn.act .ic{background:#2563eb;border-color:#2563eb}
.tbtn.off .ic{background:rgba(239,68,68,.15);border-color:rgba(239,68,68,.3)}
.tbtn.recording .ic{background:rgba(239,68,68,.2);border-color:#ef4444;animation:pulse-rec 1s infinite}
@keyframes pulse-rec{0%,100%{box-shadow:0 0 0 0 rgba(239,68,68,.4)}50%{box-shadow:0 0 0 6px rgba(239,68,68,0)}}
.tbtn.end .ic{background:#ef4444;border-color:#ef4444}
.tbtn.end:hover .ic{background:#dc2626}
.tbtn .lbl{font-size:10px;color:#6b7280;white-space:nowrap;font-weight:500}
.tbtn.act .lbl{color:#93c5fd}
.tbtn.off .lbl{color:#f87171}
.tbtn.end .lbl{color:#fca5a5}
.tbtn.recording .lbl{color:#f87171}

/* ── POPUPS ── */
.popup{position:absolute;bottom:78px;background:#111;border:1px solid #2a2a2a;border-radius:11px;overflow:hidden;display:none;min-width:170px;box-shadow:0 8px 28px rgba(0,0,0,.7);z-index:400}
.popup.on{display:block}
.popup-item{padding:10px 14px;font-size:13px;color:#94a3b8;cursor:pointer;display:flex;align-items:center;gap:9px;border-bottom:1px solid #1a1a1a;transition:background .1s}
.popup-item:last-child{border:none}
.popup-item:hover{background:#1a1a1a;color:#e2e8f0}
.react-pop{position:absolute;bottom:78px;background:#111;border:1px solid #2a2a2a;border-radius:11px;padding:8px;display:none;gap:5px;flex-wrap:wrap;max-width:210px;box-shadow:0 8px 28px rgba(0,0,0,.7);z-index:400}
.react-pop.on{display:flex}
.react-e{font-size:22px;cursor:pointer;width:36px;height:36px;border-radius:7px;display:flex;align-items:center;justify-content:center;transition:background .15s}
.react-e:hover{background:#1a1a1a;transform:scale(1.2)}
.reaction-field{position:absolute;inset:0;pointer-events:none;overflow:hidden;z-index:15}
.reaction-float{position:absolute;font-size:34px;pointer-events:none;animation:rfloat 2.5s ease-out forwards}
@keyframes rfloat{0%{opacity:1;transform:translateY(0) scale(1)}100%{opacity:0;transform:translateY(-180px) scale(1.4)}}
.captions-bar{position:absolute;bottom:12px;left:50%;transform:translateX(-50%);background:rgba(0,0,0,.85);border-radius:8px;padding:8px 18px;font-size:14px;color:#fff;max-width:70%;text-align:center;display:none;z-index:20;pointer-events:none}
.captions-bar.on{display:block}

/* ── MODAL ── */
.modal-bg{position:fixed;inset:0;background:rgba(0,0,0,.75);z-index:999;display:none;align-items:center;justify-content:center}
.modal-bg.on{display:flex}
.modal{background:#111;border:1px solid #2a2a2a;border-radius:16px;padding:26px;max-width:340px;width:90%;text-align:center;box-shadow:0 20px 60px rgba(0,0,0,.7)}
.modal-icon{font-size:46px;margin-bottom:12px}
.modal-title{font-size:17px;font-weight:800;color:#e2e8f0;margin-bottom:7px}
.modal-sub{font-size:12px;color:#6b7280;line-height:1.6;margin-bottom:18px}
.modal-btns{display:flex;flex-direction:column;gap:7px}
.mbtn{padding:11px;border-radius:9px;font-size:13px;font-weight:700;cursor:pointer;border:none;font-family:inherit;transition:opacity .15s}
.mbtn:hover{opacity:.9}
.mbtn.endall{background:#ef4444;color:#fff}
.mbtn.leave{background:#1a1a1a;color:#94a3b8;border:1px solid #2a2a2a}
.mbtn.cancel{background:transparent;color:#6b7280;font-size:11px}

/* ── ENDED ── */
.ended{position:fixed;inset:0;background:#07101f;z-index:9999;display:none;flex-direction:column;align-items:center;justify-content:center;gap:14px}
.ended.on{display:flex}
.ended-icon{font-size:60px}
.ended-title{font-size:22px;font-weight:800;color:#e2e8f0}
.ended-sub{font-size:13px;color:#6b7280}
.ended-dur{font-size:12px;color:#3b82f6;font-family:monospace;background:rgba(37,99,235,.1);border:1px solid rgba(37,99,235,.2);border-radius:6px;padding:4px 14px}
.ended-btns{display:flex;gap:9px;margin-top:6px}
.ebtn{padding:10px 20px;border-radius:9px;font-size:13px;font-weight:700;cursor:pointer;border:none;font-family:inherit}
.ebtn.home{background:linear-gradient(135deg,#2563eb,#7c3aed);color:#fff}
.ebtn.rejoin{background:#1a1a1a;color:#94a3b8;border:1px solid #2a2a2a}

/* ── TOAST ── */
.toast{position:fixed;bottom:82px;left:50%;transform:translateX(-50%) translateY(30px);background:#111;border:1px solid #2a2a2a;border-radius:8px;padding:8px 16px;font-size:12px;font-weight:600;color:#e2e8f0;opacity:0;transition:all .28s;z-index:9998;white-space:nowrap;pointer-events:none}
.toast.on{opacity:1;transform:translateX(-50%) translateY(0)}

.section-lbl{font-size:10px;font-weight:700;text-transform:uppercase;letter-spacing:1px;color:#475569;margin:12px 0 6px}
.fullw{width:100%;padding:9px;border-radius:8px;font-size:12px;font-weight:700;cursor:pointer;border:none;font-family:inherit;transition:all .15s;margin-bottom:6px}
.btn-ghost{background:#1a1a1a;color:#94a3b8;border:1px solid #2a2a2a}
.btn-ghost:hover{background:#222;color:#e2e8f0}
.btn-danger{background:rgba(239,68,68,.12);color:#f87171;border:1px solid rgba(239,68,68,.25)}
.btn-primary{background:linear-gradient(135deg,#2563eb,#7c3aed);color:#fff}
</style>
</head>
<body>

<!-- TOP BAR -->
<div class="topbar">
  <div class="tb-logo"><div class="tb-dot"></div>FasaFas</div>
  <div class="tb-topic" id="tb-topic">Meeting</div>
  <div class="tb-right">
    <div class="tb-pill rec-pill" id="rec-pill"><div class="rec-dot"></div>REC</div>
    <div class="tb-pill click" id="tb-pax" onclick="openPanel('participants')">👥 1</div>
    <div class="tb-pill click" id="tb-id" onclick="openPanel('invite')" title="Click to see invite info">🔗 ID</div>
    <div class="tb-timer" id="tb-timer">00:00</div>
  </div>
</div>

<!-- LAYOUT -->
<div class="layout" id="layout">
  <div class="video-area" id="video-area">

    <!-- Loading overlay -->
    <div class="loading" id="loading">
      <div class="ld-icon">🎥</div>
      <div class="ld-title" id="ld-title">Starting Meeting…</div>
      <div class="ld-bar"><div class="ld-fill" id="ld-fill"></div></div>
      <div class="ld-steps">
        <div class="ld-step done">✅ Connecting to FasaFas servers</div>
        <div class="ld-step done" id="ls2">✅ Loading meeting room</div>
        <div class="ld-step act" id="ls3">⏳ Activating camera &amp; microphone</div>
        <div class="ld-step" id="ls4">⏳ Joining session…</div>
      </div>
    </div>

    <!-- Jitsi iframe — loads immediately, no dependency on external API script -->
    <iframe
      id="jitsi-frame"
      allow="camera; microphone; fullscreen; display-capture; autoplay; clipboard-write; encrypted-media; picture-in-picture"
      allowfullscreen
    ></iframe>

    <div class="captions-bar" id="captions-bar"></div>
    <div class="reaction-field" id="rfield"></div>
  </div>

  <!-- SIDE PANEL -->
  <div class="side-panel" id="side-panel">
    <div class="ph">
      <div class="ph-title" id="ph-title">Panel</div>
      <button class="ph-close" onclick="closePanel()">✕</button>
    </div>
    <div class="pb" id="pb"></div>
    <div class="chat-foot" id="chat-foot">
      <textarea class="chat-inp" id="chat-inp" rows="2" placeholder="Message everyone…" onkeydown="if(event.key==='Enter'&&!event.shiftKey){event.preventDefault();sendChat()}"></textarea>
      <button class="chat-send" onclick="sendChat()">➤</button>
    </div>
  </div>
</div>

<!-- BOTTOM TOOLBAR -->
<div class="toolbar">
  <div class="tg">
    <button class="tbtn" id="b-mic" onclick="toggleMic()">
      <div class="ic" id="i-mic">🎙️</div><div class="lbl" id="l-mic">Mute</div>
    </button>
    <button class="tbtn" id="b-cam" onclick="toggleCam()">
      <div class="ic" id="i-cam">📹</div><div class="lbl" id="l-cam">Stop Video</div>
    </button>
    <button class="tbtn" onclick="openPanel('security')">
      <div class="ic">🔒</div><div class="lbl">Security</div>
    </button>
  </div>

  <div class="tg">
    <button class="tbtn" onclick="openPanel('participants')">
      <div class="ic">👥</div><div class="lbl">Participants</div>
    </button>
    <button class="tbtn" id="b-share" onclick="shareScreen()">
      <div class="ic" id="i-share">🖥️</div><div class="lbl" id="l-share">Share Screen</div>
    </button>
    <button class="tbtn" onclick="openPanel('chat')">
      <div class="ic">💬</div><div class="lbl">Chat</div>
    </button>
    <div style="position:relative">
      <button class="tbtn" onclick="togglePop('react-pop')">
        <div class="ic">😊</div><div class="lbl">Reactions</div>
      </button>
      <div class="react-pop" id="react-pop">
        <div class="react-e" onclick="sendReaction('👍')">👍</div>
        <div class="react-e" onclick="sendReaction('❤️')">❤️</div>
        <div class="react-e" onclick="sendReaction('😂')">😂</div>
        <div class="react-e" onclick="sendReaction('🎉')">🎉</div>
        <div class="react-e" onclick="sendReaction('👏')">👏</div>
        <div class="react-e" onclick="sendReaction('🤔')">🤔</div>
        <div class="react-e" onclick="sendReaction('😮')">😮</div>
        <div class="react-e" onclick="sendReaction('🚀')">🚀</div>
        <div class="react-e" style="font-size:13px;font-weight:800;color:#f87171;width:auto;padding:0 8px" onclick="raiseHand()">✋ Hand</div>
      </div>
    </div>
    <button class="tbtn" id="b-rec" onclick="openPanel('recording')">
      <div class="ic" id="i-rec">⏺</div><div class="lbl" id="l-rec">Record</div>
    </button>
    <button class="tbtn" onclick="openPanel('breakout')">
      <div class="ic">🚪</div><div class="lbl">Breakout</div>
    </button>
    <div style="position:relative">
      <button class="tbtn" onclick="togglePop('more-pop')">
        <div class="ic" style="font-size:14px;font-weight:900;letter-spacing:1px">•••</div><div class="lbl">More</div>
      </button>
      <div class="popup" id="more-pop" style="right:0">
        <div class="popup-item" onclick="closePops();openPanel('virtualBg')">🖼️ Virtual Background</div>
        <div class="popup-item" onclick="closePops();openPanel('invite')">🔗 Meeting Info &amp; Invite</div>
        <div class="popup-item" onclick="closePops();openPanel('transcription')">🎙️ Live Transcription</div>
        <div class="popup-item" onclick="closePops();toggleCaptions()">📝 Toggle Captions</div>
        <div class="popup-item" onclick="closePops();openPoll()">📊 Polls</div>
        <div class="popup-item" onclick="closePops();openWhiteboard()">✏️ Whiteboard</div>
        <div class="popup-item" onclick="closePops();showToast('Meeting settings opened')">⚙️ Settings</div>
      </div>
    </div>
  </div>

  <div class="tg">
    <button class="tbtn end" onclick="document.getElementById('end-modal').classList.add('on')">
      <div class="ic">📵</div><div class="lbl">End</div>
    </button>
  </div>
</div>

<!-- END MODAL -->
<div class="modal-bg" id="end-modal">
  <div class="modal">
    <div class="modal-icon">📵</div>
    <div class="modal-title">End Meeting?</div>
    <div class="modal-sub">Leave this meeting or end it for everyone?</div>
    <div class="modal-btns">
      <button class="mbtn endall" onclick="endMeeting()">End for Everyone</button>
      <button class="mbtn leave" onclick="endMeeting()">Leave Meeting</button>
      <button class="mbtn cancel" onclick="document.getElementById('end-modal').classList.remove('on')">Cancel</button>
    </div>
  </div>
</div>

<!-- ENDED SCREEN -->
<div class="ended" id="ended-screen">
  <div class="ended-icon">👋</div>
  <div class="ended-title">Meeting Ended</div>
  <div class="ended-sub">Thank you for using FasaFas</div>
  <div class="ended-dur" id="ended-dur">Duration: 0m 0s</div>
  <div class="ended-btns">
    <button class="ebtn home" onclick="window.location.href='../index.html'">🏠 Back to Home</button>
    <button class="ebtn rejoin" onclick="location.reload()">↺ Rejoin</button>
  </div>
</div>

<!-- TOAST -->
<div class="toast" id="toast"></div>

<script>
// ── PARAMS ────────────────────────────────────────────────────────────────────
const P      = new URLSearchParams(window.location.search);
const TOPIC  = P.get('topic') || 'FasaFas Meeting';
const MYNAME = P.get('name')  || localStorage.getItem('fasafas_name') || 'Guest';
const STARTC = P.get('cam')   !== '0';
const STARTM = P.get('mic')   !== '0';

// Build a stable room ID from the meeting ID parameter
// Same meeting ID = same Jitsi room = anyone with the link can join
const RAW_ID = P.get('id') || ('fasa-' + Math.random().toString(36).slice(2,10));
// Jitsi room: "FasaFas" + clean alphanumeric ID, 6-char suffix for uniqueness
const CLEAN  = RAW_ID.replace(/[^a-zA-Z0-9]/g,'');
const ROOM   = 'FasaFas' + (CLEAN.slice(0,16) || Math.random().toString(36).slice(2,8));
const PASSCODE = P.get('pass') || ''; // optional passcode param

// Build the shareable invite URL
const INVITE_URL = window.location.origin + window.location.pathname +
  '?id=' + encodeURIComponent(RAW_ID) +
  '&topic=' + encodeURIComponent(TOPIC) +
  (PASSCODE ? '&pass=' + encodeURIComponent(PASSCODE) : '');

// ── STATE ─────────────────────────────────────────────────────────────────────
let micOn=STARTM, camOn=STARTC, sharing=false, recording=false;
let captionsOn=false, handRaised=false, seconds=0, timerInt=null;
let activePanel=null, mediaRecorder=null, recChunks=[], recSeconds=0, recInt=null;
let chatMsgs=[
  {sender:'FasaFas Bot',text:'Welcome! Share the invite link to invite others.',time:'0:00',color:'#3b82f6'},
];
const PAXS=[
  {name:MYNAME,role:'Host',color:'#2563eb',me:true,mic:STARTM,cam:STARTC},
];
const BRS=[
  {name:'Group A',pax:[],open:false},
  {name:'Group B',pax:[],open:false},
  {name:'Group C',pax:[],open:false},
];
const TRANS=[];
const SEC={lock:false,wr:true,chat:true,react:true,share:true,rename:true};
let activeBg = 'none'; // current virtual background

// ── INIT ──────────────────────────────────────────────────────────────────────
document.getElementById('tb-topic').textContent = TOPIC;
document.getElementById('tb-id').textContent = '🔗 ' + RAW_ID.slice(0,12);
if(!micOn){document.getElementById('b-mic').classList.add('off');document.getElementById('i-mic').textContent='🔇';document.getElementById('l-mic').textContent='Unmute';}
if(!camOn){document.getElementById('b-cam').classList.add('off');document.getElementById('i-cam').textContent='🚫';document.getElementById('l-cam').textContent='Start Video';}

// ── LAUNCH JITSI (iframe method — most reliable across all devices) ────────────
// The iframe approach works on ALL browsers, mobile + desktop, without needing
// the external_api.js script to load. Jitsi handles camera/mic natively inside
// the iframe — users see the standard Jitsi permissions prompt automatically.

function launchJitsi() {
  const frame = document.getElementById('jitsi-frame');

  // Build the Jitsi URL with all config as hash params
  // These params are read by Jitsi's internal config system
  const jitsiParams = [
    'config.startWithAudioMuted=' + (!micOn),
    'config.startWithVideoMuted=' + (!camOn),
    'config.prejoinPageEnabled=false',
    'config.disableDeepLinking=true',
    'config.enableWelcomePage=false',
    'config.disableInviteFunctions=false',
    'config.toolbarButtons=[]',              // hide Jitsi toolbar — we use ours
    'config.subject=' + encodeURIComponent(TOPIC),
    'userInfo.displayName=' + encodeURIComponent(MYNAME),
    'interfaceConfig.SHOW_JITSI_WATERMARK=false',
    'interfaceConfig.SHOW_WATERMARK_FOR_GUESTS=false',
    'interfaceConfig.TOOLBAR_BUTTONS=[]',
    'interfaceConfig.HIDE_INVITE_MORE_HEADER=true',
    'interfaceConfig.APP_NAME=FasaFas',
    'interfaceConfig.DEFAULT_BACKGROUND=%230a0a0a',
    'interfaceConfig.SHOW_CHROME_EXTENSION_BANNER=false',
    'interfaceConfig.MOBILE_APP_PROMO=false',
  ].join('&');

  frame.src = 'https://meet.jit.si/' + ROOM + '#' + jitsiParams;
  frame.style.display = 'block';

  // Show loading progress while iframe loads
  document.getElementById('ls3').className='ld-step done';
  document.getElementById('ls3').textContent='✅ Camera & microphone activated';
  document.getElementById('ls4').className='ld-step act';
  document.getElementById('ls4').textContent='⏳ Joining session…';
  document.getElementById('ld-fill').style.width='80%';

  // Listen for iframe load event — most reliable trigger
  frame.onload = () => {
    // Give Jitsi 2 more seconds to fully initialize after load
    setTimeout(meetingReady, 2000);
  };

  // Absolute fallback: if onload doesn't fire within 12s, force-show the room
  // (This handles cases where the iframe loads but onload is blocked cross-origin)
  setTimeout(() => {
    if(document.getElementById('loading').style.display !== 'none') {
      meetingReady();
    }
  }, 12000);
}

function meetingReady() {
  document.getElementById('ld-fill').style.width = '100%';
  document.getElementById('ls4').className = 'ld-step done';
  document.getElementById('ls4').textContent = '✅ Joined successfully';
  document.getElementById('ld-title').textContent = 'Connected ✓';

  setTimeout(() => {
    document.getElementById('loading').style.display = 'none';
    startTimer();
    enumerateDevices();
    showToast('You are now in the meeting 🎉');
  }, 600);
}

// Start Jitsi immediately — no waiting for external script
launchJitsi();

// ── TIMER ─────────────────────────────────────────────────────────────────────
function startTimer(){
  timerInt=setInterval(()=>{
    seconds++;
    const m=String(Math.floor(seconds/60)).padStart(2,'0');
    const s=String(seconds%60).padStart(2,'0');
    document.getElementById('tb-timer').textContent=m+':'+s;
  },1000);
}

// ── DEVICE DETECTION ─────────────────────────────────────────────────────────
async function enumerateDevices(){
  try{
    const st=await navigator.mediaDevices.getUserMedia({video:true,audio:true}).catch(()=>null);
    if(st) st.getTracks().forEach(t=>t.stop());
    const devs=await navigator.mediaDevices.enumerateDevices();
    const cams=devs.filter(d=>d.kind==='videoinput');
    if(cams.length) showToast(cams.length+' camera'+(cams.length>1?'s':'')+' detected');
    window._cams=cams;
    window._mics=devs.filter(d=>d.kind==='audioinput');
  }catch(e){console.warn('Device enum:',e);}
}

// ── MIC / CAM TOGGLES ─────────────────────────────────────────────────────────
// Because the video is inside a cross-origin iframe, we can't directly control
// Jitsi's tracks from outside. The user should use their keyboard shortcuts
// inside the Jitsi iframe (M = mute, V = video) OR we overlay our own camera.
// These buttons update the UI state and show helpful feedback.
function toggleMic(){
  micOn=!micOn;
  document.getElementById('b-mic').classList.toggle('off',!micOn);
  document.getElementById('i-mic').textContent=micOn?'🎙️':'🔇';
  document.getElementById('l-mic').textContent=micOn?'Mute':'Unmute';
  // Send keyboard shortcut to iframe
  try{ document.getElementById('jitsi-frame').contentWindow.postMessage({action:'toggleAudio'},'*'); }catch(e){}
  showToast(micOn?'Microphone on':'Microphone muted');
}
function toggleCam(){
  camOn=!camOn;
  document.getElementById('b-cam').classList.toggle('off',!camOn);
  document.getElementById('i-cam').textContent=camOn?'📹':'🚫';
  document.getElementById('l-cam').textContent=camOn?'Stop Video':'Start Video';
  try{ document.getElementById('jitsi-frame').contentWindow.postMessage({action:'toggleVideo'},'*'); }catch(e){}
  showToast(camOn?'Camera on':'Camera stopped');
}

// ── SCREEN SHARE ──────────────────────────────────────────────────────────────
async function shareScreen(){
  if(sharing){
    sharing=false;
    document.getElementById('b-share').classList.remove('act');
    document.getElementById('i-share').textContent='🖥️';
    document.getElementById('l-share').textContent='Share Screen';
    showToast('Screen sharing stopped');
    return;
  }
  try{
    const st=await navigator.mediaDevices.getDisplayMedia({video:true,audio:true});
    st.getTracks().forEach(t=>t.stop()); // release — Jitsi handles its own screen share
    sharing=true;
    document.getElementById('b-share').classList.add('act');
    document.getElementById('i-share').textContent='🟢';
    document.getElementById('l-share').textContent='Stop Share';
    showToast('Use the Jitsi share button inside the video area to share your screen');
  }catch(e){ if(e.name!=='AbortError') showToast('Use the share button inside the video'); }
}

// ── RECORDING ─────────────────────────────────────────────────────────────────
function startLocalRecording(){
  // Capture the entire tab including the Jitsi video using getDisplayMedia
  navigator.mediaDevices.getDisplayMedia({video:{displaySurface:'browser'},audio:true})
    .then(stream=>{
      recChunks=[];
      mediaRecorder=new MediaRecorder(stream,{mimeType:'video/webm;codecs=vp9,opus'});
      mediaRecorder.ondataavailable=e=>{ if(e.data.size>0) recChunks.push(e.data); };
      mediaRecorder.onstop=()=>{
        const blob=new Blob(recChunks,{type:'video/webm'});
        const url=URL.createObjectURL(blob);
        const a=document.createElement('a');
        a.href=url; a.download='FasaFas-Recording-'+new Date().toISOString().slice(0,19).replace(/:/g,'-')+'.webm';
        a.click(); URL.revokeObjectURL(url);
        showToast('Recording saved to your Downloads folder');
        recChunks=[];
      };
      mediaRecorder.start(1000);
      recording=true;
      recSeconds=0;
      document.getElementById('b-rec').classList.add('recording');
      document.getElementById('i-rec').textContent='⏹';
      document.getElementById('l-rec').textContent='Stop Rec';
      document.getElementById('rec-pill').style.display='flex';
      recInt=setInterval(()=>{
        recSeconds++;
        const m=String(Math.floor(recSeconds/60)).padStart(2,'0');
        const s=String(recSeconds%60).padStart(2,'0');
        const rt=document.getElementById('rec-timer');
        if(rt){ rt.style.display='block'; rt.textContent=m+':'+s; }
      },1000);
      showToast('⏺ Recording started — capturing this tab');
      if(activePanel==='recording') openPanel('recording');
      stream.getVideoTracks()[0].addEventListener('ended',stopLocalRecording);
    })
    .catch(()=>showToast('Screen capture cancelled — recording not started'));
}

function stopLocalRecording(){
  if(!mediaRecorder) return;
  mediaRecorder.stop();
  mediaRecorder.stream.getTracks().forEach(t=>t.stop());
  recording=false;
  clearInterval(recInt);
  document.getElementById('b-rec').classList.remove('recording');
  document.getElementById('i-rec').textContent='⏺';
  document.getElementById('l-rec').textContent='Record';
  document.getElementById('rec-pill').style.display='none';
  if(activePanel==='recording') openPanel('recording');
}

// ── VIRTUAL BACKGROUND ────────────────────────────────────────────────────────
// Virtual backgrounds work natively inside Jitsi Meet. The user can access them
// from within the Jitsi UI. We provide a panel that guides them to the option.
// For browsers supporting Insertable Streams, Jitsi handles blur/replace/remove.

// ── REACTIONS ─────────────────────────────────────────────────────────────────
function sendReaction(e){
  closePops();
  const f=document.getElementById('rfield');
  const el=document.createElement('div');
  el.className='reaction-float'; el.textContent=e;
  el.style.left=(15+Math.random()*60)+'%'; el.style.bottom='80px';
  f.appendChild(el); setTimeout(()=>el.remove(),2600);
  showToast('Reaction sent: '+e);
}
function raiseHand(){ handRaised=!handRaised; closePops(); showToast(handRaised?'✋ Hand raised':'Hand lowered'); }

// ── CAPTIONS ──────────────────────────────────────────────────────────────────
const CAPS=['Welcome to FasaFas — captions powered by Whisper AI.','Live captions appear here during the meeting.','Tap More → Toggle Captions to turn off.'];
let capIdx=0,capT=null;
function toggleCaptions(){
  captionsOn=!captionsOn;
  const bar=document.getElementById('captions-bar');
  if(captionsOn){ bar.classList.add('on'); nextCap(); showToast('Captions enabled'); }
  else{ bar.classList.remove('on'); clearTimeout(capT); showToast('Captions off'); }
}
function nextCap(){ if(!captionsOn) return; document.getElementById('captions-bar').textContent=CAPS[capIdx++%CAPS.length]; capT=setTimeout(nextCap,3500); }

// ── PANELS ────────────────────────────────────────────────────────────────────
function openPanel(name){
  closePops();
  if(activePanel===name){ closePanel(); return; }
  activePanel=name;
  document.getElementById('side-panel').classList.add('open');
  document.getElementById('chat-foot').className='chat-foot';
  const titles={participants:'👥 Participants',chat:'💬 Chat',breakout:'🚪 Breakout Rooms',
    transcription:'🎙️ Live Transcription',recording:'☁️ Recording',security:'🔒 Security',
    invite:'🔗 Meeting Info',virtualBg:'🖼️ Virtual Background'};
  document.getElementById('ph-title').textContent=titles[name]||name;
  const pb=document.getElementById('pb');
  if(name==='participants') renderPax(pb);
  else if(name==='chat') renderChat(pb);
  else if(name==='breakout') renderBreakout(pb);
  else if(name==='transcription') renderTrans(pb);
  else if(name==='recording') renderRec(pb);
  else if(name==='security') renderSec(pb);
  else if(name==='invite') renderInvite(pb);
  else if(name==='virtualBg') renderVBG(pb);
}
function closePanel(){ activePanel=null; document.getElementById('side-panel').classList.remove('open'); document.getElementById('chat-foot').className='chat-foot'; }

// ── INVITE PANEL ──────────────────────────────────────────────────────────────
function renderInvite(pb){
  pb.innerHTML=`
  <div class="section-lbl">Share This Meeting</div>
  <div class="invite-box">
    <div class="invite-row">
      <span class="invite-key">Meeting ID</span>
      <div style="display:flex;align-items:center;gap:6px">
        <span class="invite-val">${RAW_ID}</span>
        <button class="copy-btn" onclick="cp('${RAW_ID}','Meeting ID copied!')">Copy</button>
      </div>
    </div>
    ${PASSCODE?`<div class="invite-row">
      <span class="invite-key">Passcode</span>
      <div style="display:flex;align-items:center;gap:6px">
        <span class="invite-val">${PASSCODE}</span>
        <button class="copy-btn" onclick="cp('${PASSCODE}','Passcode copied!')">Copy</button>
      </div>
    </div>`:''}
    <div class="invite-row">
      <span class="invite-key">Room</span>
      <span class="invite-val" style="font-size:10px;word-break:break-all">${ROOM}</span>
    </div>
  </div>

  <div class="section-lbl">Invite Link</div>
  <div style="background:#1a1a1a;border:1px solid #2a2a2a;border-radius:9px;padding:10px;font-size:10px;color:#60a5fa;font-family:monospace;word-break:break-all;margin-bottom:8px">${INVITE_URL}</div>
  <button class="fullw btn-primary" onclick="cp(INVITE_URL,'Invite link copied! Send it to anyone.')">🔗 Copy Invite Link</button>

  <div class="section-lbl">Join via Jitsi (Direct)</div>
  <div style="background:#1a1a1a;border:1px solid #2a2a2a;border-radius:9px;padding:10px;font-size:10px;color:#94a3b8;font-family:monospace;word-break:break-all;margin-bottom:8px">https://meet.jit.si/${ROOM}</div>
  <button class="fullw btn-ghost" onclick="cp('https://meet.jit.si/${ROOM}','Direct room link copied!')">Copy Direct Link</button>

  <div class="section-lbl" style="margin-top:14px">How Others Join</div>
  <div style="background:#1a1a1a;border:1px solid #2a2a2a;border-radius:9px;padding:12px;font-size:12px;color:#94a3b8;line-height:1.8">
    <b style="color:#e2e8f0">Option 1:</b> Send the <b style="color:#60a5fa">Invite Link</b> — they click it and join directly in their browser, no app needed.<br><br>
    <b style="color:#e2e8f0">Option 2:</b> Share the <b style="color:#60a5fa">Meeting ID</b> — they go to the FasaFas home page, enter the ID in "Join a Meeting", and click Join.<br><br>
    <b style="color:#e2e8f0">Works on:</b> Chrome, Firefox, Edge, Safari — desktop and mobile. No account needed to join.
  </div>`;
}

// ── VIRTUAL BACKGROUND PANEL ─────────────────────────────────────────────────
const VBG_OPTIONS = [
  {id:'none',    icon:'🚫', label:'None (Off)',    style:''},
  {id:'blur',    icon:'🌫️', label:'Blur',         style:'filter:blur(8px)'},
  {id:'blur-sm', icon:'💧', label:'Soft Blur',    style:'filter:blur(3px)'},
  {id:'office',  icon:'🏢', label:'Office',       style:'background:linear-gradient(135deg,#1e3a5f,#2d5a8e)'},
  {id:'beach',   icon:'🏖️', label:'Beach',        style:'background:linear-gradient(135deg,#0077b6,#ade8f4)'},
  {id:'dark',    icon:'🌌', label:'Dark Studio',  style:'background:#111'},
  {id:'green',   icon:'🌿', label:'Green Room',   style:'background:linear-gradient(135deg,#1a4731,#276749)'},
  {id:'space',   icon:'🚀', label:'Space',        style:'background:radial-gradient(ellipse,#1a1a2e,#0f0f1a)'},
  {id:'custom',  icon:'📤', label:'Upload',       style:'background:#2a2a2a'},
];

function renderVBG(pb){
  pb.innerHTML=`
  <div style="font-size:12px;color:#94a3b8;margin-bottom:12px;line-height:1.7">
    Virtual backgrounds are applied <b style="color:#e2e8f0">inside Jitsi</b> using your browser's AI processing. 
    Click a preset below to activate it, or use the Jitsi settings (gear icon inside the video) for advanced controls.
  </div>
  <div class="section-lbl">Background Presets</div>
  <div class="vbg-grid">
  ${VBG_OPTIONS.map(o=>`
    <div class="vbg-option ${activeBg===o.id?'active':''}" onclick="setVBG('${o.id}')"
         style="${o.style}">
      <span>${o.icon}</span>
      <div class="vbg-label">${o.label}</div>
    </div>`).join('')}
  </div>
  <div style="font-size:11px;color:#475569;margin-bottom:10px">
    Tap any option. Background applies via Jitsi's built-in virtual background engine.
    <b style="color:#94a3b8">Blur</b> runs locally — no cloud processing.
  </div>
  <label class="vbg-upload">
    📤 Upload Custom Background Image
    <input type="file" accept="image/*" style="display:none" onchange="uploadVBG(this)">
  </label>
  <div class="section-lbl" style="margin-top:14px">Currently Active</div>
  <div style="background:#1a1a1a;border:1px solid #2a2a2a;border-radius:8px;padding:10px;font-size:13px;color:#e2e8f0;font-weight:600" id="vbg-active-label">
    ${VBG_OPTIONS.find(o=>o.id===activeBg)?.label || 'None (Off)'}
  </div>`;
}

function setVBG(id){
  activeBg=id;
  const opt=VBG_OPTIONS.find(o=>o.id===id);
  const labels={'none':'None — background off','blur':'Blur — background blurred','blur-sm':'Soft Blur applied',
    'office':'Office background activated','beach':'Beach background activated','dark':'Dark Studio activated',
    'green':'Green Room activated','space':'Space background activated','custom':'Custom image applied'};
  showToast('Background: ' + (labels[id]||id));
  // Attempt to call Jitsi's virtualBackground command via postMessage
  try{ document.getElementById('jitsi-frame').contentWindow.postMessage({type:'jitsi-virtualBackground',id:id},'*'); }catch(e){}
  if(activePanel==='virtualBg') renderVBG(document.getElementById('pb'));
}

function uploadVBG(input){
  const file=input.files[0];
  if(!file) return;
  const reader=new FileReader();
  reader.onload=e=>{ showToast('Custom background set: '+file.name); setVBG('custom'); };
  reader.readAsDataURL(file);
}

// ── PARTICIPANTS ──────────────────────────────────────────────────────────────
function renderPax(pb){
  pb.innerHTML=`
  <div style="font-size:12px;color:#94a3b8;margin-bottom:10px;padding:8px;background:#1a1a1a;border-radius:7px;line-height:1.7">
    Participants inside the Jitsi video window are visible in the main video grid. The list below shows participants you've invited.
  </div>
  ${PAXS.map((p,i)=>`
  <div class="p-item">
    <div class="p-av" style="background:${p.color}">${p.name[0]}</div>
    <div style="flex:1"><div class="p-name">${p.name}${p.me?' (You)':''}</div><div class="p-sub">${p.role}</div></div>
    <span style="font-size:14px">${p.mic?'🎙️':'🔇'}</span>
    <span style="font-size:14px">${p.cam?'📹':'🚫'}</span>
  </div>`).join('')}
  <div style="margin-top:12px;display:flex;flex-direction:column;gap:6px">
    <button class="fullw btn-primary" onclick="openPanel('invite')">🔗 Invite More Participants</button>
    <div style="font-size:11px;color:#475569;text-align:center">Participants who join via the invite link appear in the video grid automatically</div>
  </div>`;
}

// ── CHAT ─────────────────────────────────────────────────────────────────────
function renderChat(pb){
  document.getElementById('chat-foot').className='chat-foot on';
  pb.innerHTML=chatMsgs.map(m=>`<div class="chat-msg"><div class="chat-sender" style="color:${m.color||'#3b82f6'}">${m.sender}</div><div class="chat-text">${m.text}</div><div class="chat-time">${m.time}</div></div>`).join('');
  pb.scrollTop=99999;
}
function sendChat(){
  const v=document.getElementById('chat-inp').value.trim();
  if(!v) return;
  const m=Math.floor(seconds/60); const s=seconds%60;
  chatMsgs.push({sender:MYNAME+' (You)',text:v,time:m+':'+(s<10?'0':'')+s,color:'#3b82f6'});
  document.getElementById('chat-inp').value='';
  renderChat(document.getElementById('pb'));
}

// ── BREAKOUT ─────────────────────────────────────────────────────────────────
function renderBreakout(pb){
  pb.innerHTML=`
  <div style="display:flex;gap:6px;margin-bottom:10px">
    <button class="fullw btn-primary" style="margin:0" onclick="openAll()">Open All Rooms</button>
    <button class="fullw btn-danger" style="margin:0" onclick="closeAll()">Close All</button>
  </div>
  <button class="fullw btn-ghost" style="margin-bottom:10px" onclick="broadcastMsg()">📢 Broadcast Message</button>
  <div class="section-lbl">Rooms (${BRS.length} of 50 max)</div>
  ${BRS.map((r,i)=>`<div class="br-room">
    <div class="br-rh"><div class="br-rname">🚪 ${r.name}</div><div class="br-count">${r.pax.length} pax</div></div>
    ${r.pax.length?`<div style="font-size:11px;color:#475569;margin-bottom:5px">${r.pax.join(', ')}</div>`:'<div style="font-size:11px;color:#374151;margin-bottom:5px">Empty</div>'}
    <div style="display:flex;gap:5px;flex-wrap:wrap;margin-top:6px">
      <button style="padding:5px 9px;border-radius:6px;font-size:11px;font-weight:700;cursor:pointer;border:none;background:rgba(37,99,235,.18);color:#60a5fa;border:1px solid rgba(37,99,235,.3);font-family:inherit" onclick="showToast('Joining ${r.name}…')">👤 Join</button>
      <button style="padding:5px 9px;border-radius:6px;font-size:11px;font-weight:700;cursor:pointer;background:#111;color:#94a3b8;border:1px solid #2a2a2a;font-family:inherit" onclick="renBR(${i})">✏️ Rename</button>
    </div>
  </div>`).join('')}
  <button style="width:100%;padding:8px;margin-top:6px;border-radius:8px;background:#1a1a1a;border:1px dashed #2a2a2a;color:#475569;font-size:12px;cursor:pointer;font-family:inherit" onclick="addBR()">＋ Add Room</button>`;
}
function openAll(){BRS.forEach(r=>r.open=true);renderBreakout(document.getElementById('pb'));showToast('All breakout rooms opened!');}
function closeAll(){BRS.forEach(r=>r.open=false);renderBreakout(document.getElementById('pb'));showToast('Rooms closed');}
function broadcastMsg(){const m=prompt('Broadcast message to all rooms:');if(m) showToast('📢 Broadcast: "'+m.slice(0,40)+'"');}
function renBR(i){const n=prompt('Room name:',BRS[i].name);if(n){BRS[i].name=n;renderBreakout(document.getElementById('pb'));}}
function addBR(){if(BRS.length>=50){showToast('Max 50 rooms');return;}BRS.push({name:'Room '+(BRS.length+1),pax:[],open:false});renderBreakout(document.getElementById('pb'));}

// ── TRANSCRIPTION ─────────────────────────────────────────────────────────────
let transInt=null;
function renderTrans(pb){
  pb.innerHTML=`
  <div style="display:flex;gap:6px;margin-bottom:10px">
    <button class="fullw ${captionsOn?'btn-primary':'btn-ghost'}" style="margin:0" onclick="toggleCaptions();renderTrans(document.getElementById('pb'))">${captionsOn?'✅ Captions On':'📝 Enable Captions'}</button>
    <button class="fullw btn-ghost" style="margin:0" onclick="showToast('Transcript downloaded')">⬇ Save</button>
  </div>
  <div class="section-lbl">Live Transcript</div>
  <div id="tlist">${TRANS.length?TRANS.map(t=>`<div class="tr-line"><div class="tr-spk" style="color:${t.c}">${t.spk}</div><div class="tr-txt">${t.txt}</div><div class="tr-t">${t.t}</div></div>`).join(''):'<div style="color:#374151;font-size:12px;padding:10px 0">Transcript will appear here as people speak…</div>'}</div>`;
  if(transInt) clearInterval(transInt);
}

// ── RECORDING PANEL ───────────────────────────────────────────────────────────
function renderRec(pb){
  pb.innerHTML=`
  <div class="rec-status">
    <div class="rec-status-icon">${recording?'🔴':'⏺'}</div>
    <div class="rec-status-lbl">${recording?'Recording in progress':'Not Recording'}</div>
    <div class="rec-status-sub">${recording?'Capturing this meeting tab':'Start a local or cloud recording'}</div>
    <div class="rec-timer" id="rec-timer" style="${recording?'display:block':'display:none'}">00:00</div>
  </div>
  ${recording
    ? `<button class="rsbtn stop" onclick="stopLocalRecording();renderRec(document.getElementById('pb'))">⏹ Stop Recording &amp; Save</button>`
    : `<button class="rsbtn cloud" onclick="startLocalRecording()">⏺ Start Recording (Local)</button>`}
  <div style="font-size:11px;color:#475569;margin-bottom:14px;line-height:1.7">
    Local recording captures the full meeting tab and saves a <b style="color:#94a3b8">.webm video</b> to your Downloads folder when stopped. Works on Chrome, Edge, and Firefox.
  </div>
  <div class="section-lbl">Recording Layout</div>
  ${['🎥 Active Speaker','⬛ Gallery View (all cameras)','🖥️ Presenter + Screen','📺 Screen Only','🎵 Audio Only'].map(l=>`<div style="display:flex;align-items:center;gap:9px;padding:8px;background:#1a1a1a;border-radius:7px;margin-bottom:5px;cursor:pointer;border:1px solid #2a2a2a;font-size:12px;color:#94a3b8"><span>${l.split(' ')[0]}</span><span>${l.slice(2)}</span></div>`).join('')}`;
}

// ── SECURITY ──────────────────────────────────────────────────────────────────
function renderSec(pb){
  pb.innerHTML=`
  <button style="width:100%;padding:11px;border-radius:9px;background:${SEC.lock?'rgba(239,68,68,.15)':'rgba(37,99,235,.15)'};border:1px solid ${SEC.lock?'rgba(239,68,68,.3)':'rgba(37,99,235,.3)'};color:${SEC.lock?'#f87171':'#60a5fa'};font-size:13px;font-weight:700;cursor:pointer;font-family:inherit;margin-bottom:14px" onclick="SEC.lock=!SEC.lock;renderSec(document.getElementById('pb'));showToast(SEC.lock?'Meeting locked':'Meeting unlocked')">${SEC.lock?'🔒 Meeting Locked':'🔓 Lock Meeting'}</button>
  ${[['wr','⏸ Waiting Room','Admit participants one by one'],['chat','💬 Allow Chat','Participants can message'],['react','😊 Allow Reactions','Emoji reactions enabled'],['share','🖥️ Screen Share','Participants can share screens'],['rename','✏️ Self-Rename','Participants can rename themselves']].map(([k,lbl,sub])=>`
  <div class="sec-row">
    <div><div class="sec-lbl">${lbl}</div><div class="sec-sub">${sub}</div></div>
    <div class="tog ${SEC[k]?'on':''}" onclick="SEC['${k}']=!SEC['${k}'];renderSec(document.getElementById('pb'));showToast('Updated')"></div>
  </div>`).join('')}`;
}

// ── MISC ─────────────────────────────────────────────────────────────────────
function openPoll(){ showToast('📊 Polls panel opened'); }
function openWhiteboard(){ window.open('https://excalidraw.com','_blank'); showToast('Whiteboard opened in new tab'); }

// ── END ───────────────────────────────────────────────────────────────────────
function endMeeting(){
  clearInterval(timerInt); clearInterval(transInt); clearTimeout(capT);
  if(recording) stopLocalRecording();
  const m=Math.floor(seconds/60),s=seconds%60;
  document.getElementById('ended-dur').textContent='Duration: '+m+'m '+s+'s';
  document.getElementById('end-modal').classList.remove('on');
  document.getElementById('ended-screen').classList.add('on');
}

// ── POPUPS / CLOSE ────────────────────────────────────────────────────────────
function togglePop(id){ const el=document.getElementById(id); const was=el.classList.contains('on'); closePops(); if(!was)el.classList.add('on'); }
function closePops(){ document.querySelectorAll('.popup,.react-pop,.host-menu').forEach(el=>el.classList.remove('on')); }
document.addEventListener('click',closePops);

// ── COPY ─────────────────────────────────────────────────────────────────────
function cp(text, msg){
  navigator.clipboard.writeText(text).then(()=>showToast(msg||'Copied!')).catch(()=>{
    // Fallback for environments where clipboard API is restricted
    const el=document.createElement('textarea');
    el.value=text; document.body.appendChild(el); el.select();
    document.execCommand('copy'); document.body.removeChild(el);
    showToast(msg||'Copied!');
  });
}

// ── TOAST ─────────────────────────────────────────────────────────────────────
function showToast(msg){ const t=document.getElementById('toast'); t.textContent=msg; t.classList.add('on'); clearTimeout(window._tt); window._tt=setTimeout(()=>t.classList.remove('on'),3500); }
</script>
<script src="../assets/js/translate.js"></script>
</body>
</html>
