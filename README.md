<!DOCTYPE html>
<html lang="id">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Rio De Ornay — Profile</title>
  <link href="https://fonts.googleapis.com/css2?family=Syne:wght@400;700;800&family=Space+Mono:wght@400;700&display=swap" rel="stylesheet"/>
  <style>
    *{box-sizing:border-box;margin:0;padding:0}
    :root{
      --bg:#0d0d0d;--card:#161616;--accent:#f5a623;--accent2:#e94e77;--accent3:#4ecdc4;
      --text:#f0ece3;--muted:#888;--border:#2a2a2a;
    }
    body{background:var(--bg);color:var(--text);font-family:'Space Mono',monospace;padding:0}
    .wrap{max-width:680px;margin:0 auto;padding:2rem 1.5rem}

    .header{display:grid;grid-template-columns:1fr auto;gap:1rem;align-items:start;margin-bottom:2.5rem;border-bottom:1px solid var(--border);padding-bottom:2rem}
    .name{font-family:'Syne',sans-serif;font-weight:800;font-size:2rem;line-height:1;letter-spacing:-1px;background:linear-gradient(135deg,var(--text) 40%,var(--accent));-webkit-background-clip:text;-webkit-text-fill-color:transparent;background-clip:text}
    .title{font-size:0.7rem;color:var(--accent3);letter-spacing:3px;text-transform:uppercase;margin-top:6px;font-family:'Space Mono',monospace}
    .origin{font-size:0.7rem;color:var(--muted);margin-top:4px}
    .status-badge{background:var(--card);border:1px solid var(--border);border-radius:6px;padding:10px 14px;text-align:right;min-width:160px}
    .status-dot{display:inline-block;width:7px;height:7px;background:#4ade80;border-radius:50%;margin-right:6px;animation:pulse 2s infinite}
    @keyframes pulse{0%,100%{opacity:1}50%{opacity:0.3}}
    .status-label{font-size:0.65rem;color:var(--accent3)}
    .status-val{font-size:0.7rem;color:var(--muted);margin-top:4px}

    .section-label{font-size:0.62rem;letter-spacing:3px;text-transform:uppercase;color:var(--muted);margin-bottom:1rem;font-family:'Syne',sans-serif}

    .project-card{background:var(--card);border:1px solid var(--border);border-radius:10px;padding:1.25rem 1.5rem;margin-bottom:1.5rem;position:relative;overflow:hidden}
    .project-card::before{content:'';position:absolute;top:0;left:0;width:3px;height:100%;background:var(--accent)}
    .project-title{font-family:'Syne',sans-serif;font-weight:700;font-size:1rem;color:var(--text)}
    .project-desc{font-size:0.72rem;color:var(--muted);margin-top:6px;line-height:1.6}
    .project-tag{display:inline-block;background:#1f1f1f;border:1px solid var(--border);border-radius:4px;padding:2px 8px;font-size:0.6rem;color:var(--accent);margin-top:8px;margin-right:4px;letter-spacing:1px}

    .facts{display:grid;grid-template-columns:1fr 1fr 1fr;gap:1rem;margin-bottom:2rem}
    .fact{background:var(--card);border:1px solid var(--border);border-radius:8px;padding:1rem;text-align:center}
    .fact-icon{font-size:1.2rem;margin-bottom:6px}
    .fact-label{font-size:0.62rem;color:var(--muted);text-transform:uppercase;letter-spacing:2px}

    .tools-grid{display:flex;flex-wrap:wrap;gap:8px;margin-bottom:2rem}
    .tool-chip{background:var(--card);border:1px solid var(--border);border-radius:6px;padding:6px 12px;font-size:0.68rem;color:var(--text);font-family:'Syne',sans-serif;font-weight:700;letter-spacing:0.5px;transition:border-color 0.2s,color 0.2s;cursor:default}
    .tool-chip:hover{border-color:var(--accent2);color:var(--accent2)}
    .tool-chip.ai{border-color:#333;color:var(--accent3)}
    .tool-chip.web{border-color:#333;color:var(--accent)}

    .contacts{display:flex;gap:10px;flex-wrap:wrap;margin-bottom:2rem}
    .contact-pill{display:flex;align-items:center;gap:8px;background:var(--card);border:1px solid var(--border);border-radius:999px;padding:8px 16px;font-size:0.7rem;color:var(--muted);cursor:pointer;transition:all 0.2s}
    .contact-pill:hover{border-color:var(--accent2);color:var(--accent2)}
    .pill-icon{width:16px;height:16px;border-radius:50%;display:flex;align-items:center;justify-content:center;font-size:10px;background:#1f1f1f}

    .footer{border-top:1px solid var(--border);padding-top:1rem;display:flex;justify-content:space-between;align-items:center}
    .footer-text{font-size:0.62rem;color:var(--muted)}
    .footer-accent{color:var(--accent);font-weight:700}
  </style>
</head>
<body>
<div class="wrap">
  <div class="header">
    <div>
      <div class="name">Rio De Ornay</div>
      <div class="title">// ML Engineer · Web Dev</div>
      <div class="origin">East Nusa Tenggara, Indonesia</div>
    </div>
    <div class="status-badge">
      <div><span class="status-dot"></span><span class="status-label">Active</span></div>
      <div class="status-val" style="font-size:0.6rem;margin-top:6px">@apriliodeornay</div>
    </div>
  </div>

  <div class="section-label">// Current project</div>
  <div class="project-card">
    <div class="project-title">ASR · Javanese Audio</div>
    <div class="project-desc">Building an Automatic Speech Recognition system for Javanese language — bridging NLP and low-resource language preservation.</div>
    <span class="project-tag">PyTorch</span>
    <span class="project-tag">TensorFlow</span>
    <span class="project-tag">OpenCV</span>
    <span class="project-tag">Scikit-learn</span>
  </div>

  <div class="section-label">// Fun facts</div>
  <div class="facts">
    <div class="fact">
      <div class="fact-icon">🍪</div>
      <div class="fact-label">Oreos</div>
    </div>
    <div class="fact">
      <div class="fact-icon">🍩</div>
      <div class="fact-label">Donuts</div>
    </div>
    <div class="fact">
      <div class="fact-icon">🎵</div>
      <div class="fact-label">RnB</div>
    </div>
  </div>

  <div class="section-label">// Stack</div>
  <div class="tools-grid">
    <span class="tool-chip ai">Python</span>
    <span class="tool-chip ai">PyTorch</span>
    <span class="tool-chip ai">TensorFlow</span>
    <span class="tool-chip ai">Scikit-learn</span>
    <span class="tool-chip ai">Pandas</span>
    <span class="tool-chip ai">Seaborn</span>
    <span class="tool-chip ai">OpenCV</span>
    <span class="tool-chip web">HTML5</span>
    <span class="tool-chip web">CSS3</span>
    <span class="tool-chip web">Bootstrap</span>
    <span class="tool-chip web">Laravel</span>
    <span class="tool-chip">Java</span>
    <span class="tool-chip">Git</span>
  </div>

  <div class="section-label">// Connect</div>
  <div class="contacts">
    <div class="contact-pill">
      <div class="pill-icon">✉</div>
      rio8i2004@gmail.com
    </div>
    <div class="contact-pill">
      <div class="pill-icon">ig</div>
      @apriliodeornay
    </div>
    <div class="contact-pill">
      <div class="pill-icon">lc</div>
      rionewbieiseng2
    </div>
  </div>

  <div class="footer">
    <span class="footer-text">Newbie from <span class="footer-accent">NTT</span> · Building cool things one commit at a time</span>
    <span class="footer-text" style="color:#333">v2025</span>
  </div>
</div>
</body>
</html>
