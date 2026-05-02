/* FasaFas Feature Pages — Shared Stylesheet */
:root{
  --bg:#07101f;--bg2:#0d1b2e;--bg3:#132039;--card:#0f1f35;
  --border:#1e3050;--accent:#2563eb;--accent2:#3b82f6;--accent3:#60a5fa;
  --green:#10b981;--amber:#f59e0b;--red:#ef4444;--purple:#8b5cf6;
  --teal:#14b8a6;--pink:#ec4899;--orange:#f97316;
  --text:#e2e8f0;--text2:#94a3b8;--text3:#475569;
  --font:'Segoe UI',system-ui,sans-serif;--mono:'Consolas','SF Mono',monospace;
}
*{box-sizing:border-box;margin:0;padding:0}
html,body{background:var(--bg);color:var(--text);font-family:var(--font);font-size:14px;line-height:1.6;overflow-x:hidden}
::-webkit-scrollbar{width:4px}::-webkit-scrollbar-thumb{background:var(--border);border-radius:4px}

/* Header */
.f-header{position:sticky;top:0;z-index:100;display:flex;align-items:center;justify-content:space-between;padding:0 32px;height:54px;background:rgba(7,16,31,.95);backdrop-filter:blur(12px);border-bottom:1px solid var(--border)}
.f-logo{font-size:18px;font-weight:900;color:var(--accent2);display:flex;align-items:center;gap:8px;cursor:pointer;letter-spacing:-.4px;text-decoration:none}
.f-logo-dot{width:7px;height:7px;border-radius:50%;background:var(--accent2);animation:pulse 2s infinite}
@keyframes pulse{0%,100%{opacity:1}50%{opacity:.4}}
.f-nav{display:flex;gap:8px;align-items:center}
.f-back{padding:6px 13px;border-radius:7px;border:1px solid var(--border);background:transparent;color:var(--text2);font-size:12px;font-weight:600;cursor:pointer;transition:all .15s;font-family:var(--font);text-decoration:none;display:inline-flex;align-items:center;gap:5px}
.f-back:hover{background:var(--bg3);color:var(--text);border-color:var(--accent2)}
.f-schedule{padding:6px 14px;border-radius:7px;background:linear-gradient(135deg,#2563eb,#7c3aed);color:#fff;font-size:12px;font-weight:700;cursor:pointer;border:none;font-family:var(--font);text-decoration:none;display:inline-flex;align-items:center;gap:5px}
.f-schedule:hover{opacity:.9}

/* Breadcrumb */
.breadcrumb{display:flex;align-items:center;gap:6px;padding:10px 32px;font-size:12px;color:var(--text3);background:var(--bg2);border-bottom:1px solid var(--border)}
.breadcrumb a{color:var(--text3);text-decoration:none;cursor:pointer}
.breadcrumb a:hover{color:var(--accent3)}
.breadcrumb .sep{color:var(--border)}
.breadcrumb .cur{color:var(--text2)}

/* Hero */
.f-hero{padding:48px 44px 40px;border-bottom:1px solid var(--border);position:relative;overflow:hidden}
.f-hero::before{content:'';position:absolute;right:-100px;top:-100px;width:400px;height:400px;border-radius:50%;background:radial-gradient(circle,var(--hero-glow,rgba(37,99,235,.12)),transparent 70%);pointer-events:none}
.hero-eyebrow{display:inline-flex;align-items:center;gap:6px;background:var(--hero-eyebrow-bg,rgba(37,99,235,.12));border:1px solid var(--hero-eyebrow-b,rgba(37,99,235,.25));border-radius:20px;padding:3px 12px;font-size:11px;color:var(--hero-eyebrow-c,var(--accent3));margin-bottom:14px;font-weight:600;letter-spacing:.3px}
.f-hero-icon{font-size:52px;margin-bottom:14px;display:block}
.f-hero-title{font-size:38px;font-weight:900;letter-spacing:-1.5px;line-height:1.1;margin-bottom:12px}
.f-hero-title em{font-style:normal;color:var(--hero-color,var(--accent2))}
.f-hero-desc{color:var(--text2);font-size:15px;max-width:620px;line-height:1.75;margin-bottom:24px}
.hero-badges{display:flex;flex-wrap:wrap;gap:8px}
.h-badge{padding:4px 12px;border-radius:6px;font-size:11px;font-weight:700;background:var(--card);border:1px solid var(--border);color:var(--text2)}

/* Tabs */
.f-tabs{display:flex;gap:0;padding:0 44px;background:var(--bg2);border-bottom:1px solid var(--border);overflow-x:auto}
.f-tab{padding:13px 20px;font-size:13px;color:var(--text3);cursor:pointer;border-bottom:2px solid transparent;white-space:nowrap;transition:all .15s;font-weight:500}
.f-tab:hover{color:var(--text2)}
.f-tab.active{color:var(--tab-color,var(--accent3));border-bottom-color:var(--tab-color,var(--accent))}

/* Content */
.f-content{padding:32px 44px;max-width:1100px}
.tab-pane{display:none}.tab-pane.active{display:block}
.ph1{font-size:20px;font-weight:800;letter-spacing:-.4px;margin-bottom:6px}
.ph2{font-size:16px;font-weight:700;letter-spacing:-.3px;margin:26px 0 12px;display:flex;align-items:center;gap:8px}
.ph2:first-child{margin-top:0}
.ph2-dot{width:7px;height:7px;border-radius:50%;background:var(--accent2);flex-shrink:0}
.ph3{font-size:11px;font-weight:700;text-transform:uppercase;letter-spacing:.8px;color:var(--accent3);margin:0 0 10px}
.pdesc{color:var(--text2);font-size:13px;margin-bottom:18px;line-height:1.7}

/* Grids */
.g2{display:grid;grid-template-columns:1fr 1fr;gap:14px}
.g3{display:grid;grid-template-columns:1fr 1fr 1fr;gap:14px}
.g4{display:grid;grid-template-columns:repeat(4,1fr);gap:12px}
@media(max-width:880px){.g3{grid-template-columns:1fr 1fr}.g4{grid-template-columns:1fr 1fr}}
@media(max-width:580px){.g2,.g3,.g4{grid-template-columns:1fr}}

/* Cards */
.card{background:var(--card);border:1px solid var(--border);border-radius:10px;padding:20px}
.stat-card{background:var(--card);border:1px solid var(--border);border-radius:10px;padding:18px}
.stat-label{font-size:10px;font-weight:700;text-transform:uppercase;letter-spacing:.8px;color:var(--text3);margin-bottom:5px}
.stat-val{font-size:26px;font-weight:800;letter-spacing:-.5px;margin-bottom:3px}
.stat-sub{font-size:11px;color:var(--text3)}

/* Feature list */
.fl{list-style:none}
.fl li{display:flex;align-items:flex-start;gap:8px;padding:6px 0;border-bottom:1px solid rgba(30,48,80,.5);font-size:13px;color:var(--text2)}
.fl li:last-child{border:none}
.fl li::before{content:'→';color:var(--accent2);font-size:11px;margin-top:3px;flex-shrink:0}

/* Spec table */
.spec{background:var(--bg3);border:1px solid var(--border);border-radius:9px;overflow:hidden;margin:10px 0}
.spec-head{background:rgba(37,99,235,.08);padding:9px 16px;font-size:12px;font-weight:700;color:var(--accent3);border-bottom:1px solid var(--border);display:flex;align-items:center;gap:8px}
.spec-row{display:grid;grid-template-columns:180px 1fr;padding:7px 16px;border-bottom:1px solid rgba(30,48,80,.4);font-size:12.5px}
.spec-row:last-child{border:none}
.spec-key{color:var(--text2);font-weight:600}
.spec-val{color:var(--text);font-family:var(--mono);font-size:12px}

/* Compare table */
.ctable{width:100%;border-collapse:collapse;font-size:12.5px;margin:8px 0}
.ctable th{background:var(--bg3);color:var(--text3);padding:9px 14px;text-align:left;font-size:11px;font-weight:700;text-transform:uppercase;letter-spacing:.5px;border-bottom:1px solid var(--border)}
.ctable td{padding:9px 14px;border-bottom:1px solid rgba(30,48,80,.4);color:var(--text2);vertical-align:top}
.ctable tr:hover td{background:rgba(19,32,57,.5)}
.ck{color:var(--green);font-weight:700}.da{color:var(--text3)}.pa{color:var(--amber)}

/* Badge */
.badge{display:inline-flex;padding:2px 9px;border-radius:5px;font-size:10px;font-weight:700}
.bg{background:rgba(16,185,129,.15);color:#6ee7b7;border:1px solid rgba(16,185,129,.25)}
.ba{background:rgba(37,99,235,.15);color:#93c5fd;border:1px solid rgba(37,99,235,.25)}
.bp{background:rgba(139,92,246,.15);color:#c4b5fd;border:1px solid rgba(139,92,246,.25)}
.bam{background:rgba(245,158,11,.15);color:#fcd34d;border:1px solid rgba(245,158,11,.25)}
.br{background:rgba(239,68,68,.15);color:#fca5a5;border:1px solid rgba(239,68,68,.25)}
.bt{background:rgba(20,184,166,.15);color:#5eead4;border:1px solid rgba(20,184,166,.25)}

/* Highlight box */
.highlight{border-radius:10px;padding:16px 18px;margin:14px 0;border-left:3px solid var(--hl-color,var(--accent))}
.highlight.blue{background:rgba(37,99,235,.08);border-color:var(--accent);color:var(--text2)}
.highlight.green{background:rgba(16,185,129,.08);border-color:var(--green);color:var(--text2)}
.highlight.amber{background:rgba(245,158,11,.08);border-color:var(--amber);color:var(--text2)}
.highlight.purple{background:rgba(139,92,246,.08);border-color:var(--purple);color:var(--text2)}
.hl-title{font-size:13px;font-weight:700;margin-bottom:4px}

/* Step list */
.steps{list-style:none;counter-reset:steps}
.steps li{counter-increment:steps;display:flex;align-items:flex-start;gap:12px;padding:10px 0;border-bottom:1px solid rgba(30,48,80,.4)}
.steps li:last-child{border:none}
.steps li::before{content:counter(steps);width:24px;height:24px;border-radius:50%;background:var(--accent);color:#fff;font-size:11px;font-weight:800;display:flex;align-items:center;justify-content:center;flex-shrink:0;margin-top:2px}
.step-title{font-size:13px;font-weight:700;margin-bottom:2px}
.step-desc{font-size:12px;color:var(--text2)}

/* API block */
.ep{display:flex;align-items:center;gap:8px;background:var(--bg3);border:1px solid var(--border);border-radius:7px;padding:9px 14px;margin:4px 0;font-family:var(--mono);font-size:12px}
.meth{padding:2px 7px;border-radius:3px;font-size:10px;font-weight:700;flex-shrink:0}
.GET{background:rgba(16,185,129,.18);color:#6ee7b7}
.POST{background:rgba(37,99,235,.18);color:#93c5fd}
.ep-path{color:var(--text2);flex:1}
.ep-note{color:var(--text3);font-size:11px}

/* Code */
.code{background:#050d18;border:1px solid var(--border);border-radius:9px;padding:16px;font-family:var(--mono);font-size:12px;line-height:1.9;overflow-x:auto;margin:10px 0;color:#a5b4fc}
.kw{color:#f472b6}.str{color:#6ee7b7}.com{color:#334d66}.fn{color:#60a5fa}

/* Feature navigation strip */
.feat-nav{display:grid;grid-template-columns:repeat(5,1fr);gap:10px;padding:18px 44px;background:var(--bg2);border-top:1px solid var(--border)}
.feat-nav-item{background:var(--card);border:1px solid var(--border);border-radius:9px;padding:12px;cursor:pointer;transition:all .15s;text-align:center;text-decoration:none;display:block}
.feat-nav-item:hover{border-color:var(--accent2);transform:translateY(-1px)}
.feat-nav-item.current{border-color:var(--accent);background:rgba(37,99,235,.1)}
.fni-icon{font-size:20px;margin-bottom:4px}
.fni-label{font-size:11px;font-weight:700;color:var(--text2)}
.feat-nav-item.current .fni-label{color:var(--accent3)}
