<!doctype html>
<html lang="de">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width,initial-scale=1,viewport-fit=cover">
<meta name="color-scheme" content="light dark">
<title>Flying 30 Doppeltor</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Barlow+Condensed:wght@600;700&family=Barlow:wght@400;500;600&display=swap" rel="stylesheet">
<style>
/* Layout: eine Spalte, Handy zuerst. Rolle, Kamerabild mit Tor, Sync und Messung, Läufe, Ergebnis. */
:root{
  --paper:#ECEFE9; --card:#F8FAF6; --ink:#14201C; --muted:#56635B; --line:#C5CCC3;
  --accent:#BF3F0F; --accent-ink:#FFFFFF; --ok:#1B7A44; --warn:#A86A00; --crit:#B42318;
  --display:'Barlow Condensed','Arial Narrow',Arial,sans-serif;
  --body:'Barlow',system-ui,-apple-system,'Segoe UI',Roboto,sans-serif;
  color-scheme:light;
}
@media (prefers-color-scheme: dark){
  :root{
    --paper:#0F1612; --card:#17201B; --ink:#E8ECE5; --muted:#9BA79E; --line:#2B372F;
    --accent:#F27A3C; --accent-ink:#1A0D05; --ok:#4CC27E; --warn:#E3B04B; --crit:#FF7A6B;
    color-scheme:dark;
  }
}
*{box-sizing:border-box}
[hidden]{display:none!important}
html,body{margin:0}
body{
  background:var(--paper); color:var(--ink); font:16px/1.45 var(--body);
  padding-left:max(16px,env(safe-area-inset-left,0px));
  padding-right:max(16px,env(safe-area-inset-right,0px));
  padding-block:max(14px,env(safe-area-inset-top,0px)) max(28px,env(safe-area-inset-bottom,0px));
  -webkit-text-size-adjust:100%;
}
.app{max-width:560px;margin:0 auto;display:flex;flex-direction:column;gap:14px}
h1,h2{font-family:var(--display);font-weight:700;margin:0;letter-spacing:.01em;text-wrap:balance}
h1{font-size:34px;line-height:1}
h2{font-size:20px;text-transform:uppercase;letter-spacing:.06em}
p{margin:0}
.sub{color:var(--muted);font-size:14px;margin-top:4px}
.top{display:flex;flex-wrap:wrap;align-items:flex-end;justify-content:space-between;gap:10px}
.chips{display:flex;flex-wrap:wrap;gap:6px}
.chip{font:600 13px/1 var(--body);padding:6px 9px;border:1px solid var(--line);border-radius:4px;color:var(--muted);background:var(--card);font-variant-numeric:tabular-nums}
.chip.ok{color:var(--ok);border-color:var(--ok)}
.chip.warn{color:var(--warn);border-color:var(--warn)}
.card{background:var(--card);border:1px solid var(--line);border-radius:6px;padding:14px;display:flex;flex-direction:column;gap:12px;min-width:0}
.row{display:flex;flex-wrap:wrap;gap:10px;align-items:center}

.roles{display:grid;grid-template-columns:1fr 1fr;gap:8px}
.roles input{position:absolute;opacity:0;pointer-events:none}
.roles label{border:2px solid var(--line);border-radius:6px;padding:10px 12px;cursor:pointer;display:flex;flex-direction:column;gap:2px;background:var(--paper)}
.roles label b{font:700 26px/1 var(--display);letter-spacing:.04em}
.roles label span{font-size:13px;color:var(--muted)}
.roles input:checked+label{border-color:var(--accent);background:var(--card);box-shadow:inset 0 0 0 1px var(--accent)}
.roles input:focus-visible+label{outline:3px solid var(--accent);outline-offset:2px}

.viewer{display:grid;place-items:center;background:#0b0f0d;border-radius:4px;overflow:hidden;position:relative}
.frame{position:relative;width:min(100%,calc(58svh * var(--ar,1.7778)));aspect-ratio:var(--ar,1.7778);max-width:100%}
.frame video,.frame canvas{position:absolute;inset:0;width:100%;height:100%;display:block}
.frame video{object-fit:fill;background:#0b0f0d}
.frame.hit{outline:4px solid var(--accent);outline-offset:-4px}
.ph{position:absolute;inset:0;display:grid;place-items:center;text-align:center;padding:16px;color:#C9D1CB;font-size:14px}

.status{display:flex;align-items:center;gap:10px;font:700 22px/1.15 var(--display);letter-spacing:.03em;text-transform:uppercase}
.dot{width:14px;height:14px;border-radius:50%;background:var(--muted);flex:none}
.dot.learning{background:var(--warn)}
.dot.armed{background:var(--ok)}
.dot.locked{background:var(--crit)}
.hint{font-size:14px;color:var(--muted)}
canvas.spark{width:100%;height:56px;display:block;background:var(--paper);border:1px solid var(--line);border-radius:4px}

.field{display:grid;grid-template-columns:1fr auto;gap:4px 10px;align-items:center}
.field label{font-weight:600;font-size:14px}
.field output{font:600 15px var(--body);font-variant-numeric:tabular-nums;color:var(--muted)}
.field input[type=range]{grid-column:1 / -1;width:100%;accent-color:var(--accent);margin:4px 0}

button{font:700 18px/1 var(--display);letter-spacing:.05em;text-transform:uppercase;padding:14px 16px;border-radius:5px;border:2px solid var(--ink);background:transparent;color:var(--ink);cursor:pointer;min-height:48px}
button.primary{background:var(--accent);border-color:var(--accent);color:var(--accent-ink)}
button:disabled{opacity:.4;cursor:not-allowed}
button:focus-visible,summary:focus-visible,textarea:focus-visible{outline:3px solid var(--accent);outline-offset:2px}
.btns{display:grid;grid-template-columns:1fr;gap:8px}
@media (min-width:430px){.btns.two{grid-template-columns:1fr 1fr}}
button.small{font-size:15px;padding:10px 12px;min-height:40px}

ol.runs{list-style:none;margin:0;padding:0;display:flex;flex-direction:column;border-top:1px solid var(--line)}
ol.runs li{display:flex;align-items:center;gap:10px;padding:8px 0;border-bottom:1px solid var(--line);font-variant-numeric:tabular-nums}
ol.runs .n{font:700 18px var(--display);width:2.2em;color:var(--muted)}
ol.runs .v{font:600 20px var(--body);flex:1}
ol.runs button{padding:6px 10px;min-height:36px;font-size:15px;border-width:1px}
.empty{color:var(--muted);font-size:14px}

textarea{width:100%;min-height:64px;resize:vertical;font:14px/1.4 ui-monospace,SFMono-Regular,Menlo,Consolas,monospace;background:var(--paper);color:var(--ink);border:1px solid var(--line);border-radius:4px;padding:8px}
.tablewrap{overflow-x:auto}
table{border-collapse:collapse;width:100%;font-variant-numeric:tabular-nums}
th,td{text-align:right;padding:8px 6px;border-bottom:1px solid var(--line);white-space:nowrap}
th:first-child,td:first-child{text-align:left}
th{font:700 14px var(--display);letter-spacing:.08em;text-transform:uppercase;color:var(--muted)}
td.t{font:700 22px var(--display);letter-spacing:.02em}
tr.best td{color:var(--accent)}
tr.best td:first-child::after{content:" ★"}
.sum{display:flex;flex-wrap:wrap;gap:6px 18px;font-size:15px}
.sum b{font:700 20px var(--display);letter-spacing:.02em}
.err{color:var(--crit);font-size:14px;font-weight:600}
.note{font-size:14px;color:var(--muted)}

details.card summary{cursor:pointer;font:700 20px var(--display);text-transform:uppercase;letter-spacing:.06em;list-style:none}
details.card summary::-webkit-details-marker{display:none}
details.card summary::after{content:"+";float:right;color:var(--muted)}
details[open].card summary::after{content:"–"}
details .body{display:flex;flex-direction:column;gap:10px;margin-top:10px;font-size:15px}
details ol,details ul{margin:0;padding-left:1.2em;display:flex;flex-direction:column;gap:6px}
@media (prefers-reduced-motion:no-preference){.frame.hit{transition:outline-color .2s}}
</style>
</head>
<body>
<main class="app">

  <header class="top">
    <div>
      <h1>Flying 30</h1>
      <p class="sub">Doppeltor mit zwei Handys</p>
    </div>
    <div class="chips" aria-live="polite">
      <span class="chip" id="chipFps">Kamera aus</span>
      <span class="chip" id="chipSync">nicht synchron</span>
    </div>
  </header>

  <section class="card" aria-labelledby="hRole">
    <h2 id="hRole">Dieses Handy ist</h2>
    <div class="roles" role="radiogroup" aria-labelledby="hRole">
      <input type="radio" name="role" id="roleS" value="S"><label for="roleS"><b>Start</b><span>Tor bei 0 m</span></label>
      <input type="radio" name="role" id="roleZ" value="Z"><label for="roleZ"><b>Ziel</b><span>Tor bei 30 m</span></label>
    </div>
  </section>

  <section class="card" aria-labelledby="hCam">
    <h2 id="hCam">Kamera und Tor</h2>
    <div class="viewer">
      <div class="frame" id="frame">
        <video id="vid" playsinline muted autoplay></video>
        <canvas id="ov"></canvas>
        <div class="ph" id="ph">Kamera ist aus. Starte sie, dann siehst du hier das Torband.</div>
      </div>
    </div>
    <div class="status"><span class="dot" id="dot"></span><span id="statusTxt">Kamera aus</span></div>
    <p class="hint" id="statusHint"></p>
    <canvas class="spark" id="spark" aria-label="Bewegung im Torband"></canvas>

    <div class="field">
      <label for="pos">Torposition im Bild</label><output id="posOut">50 %</output>
      <input type="range" id="pos" min="5" max="95" step="1">
    </div>
    <div class="field">
      <label for="yTop">Erkennung ab oben (Bildhöhe)</label><output id="yTopOut">0 %</output>
      <input type="range" id="yTop" min="0" max="90" step="1">
    </div>
    <div class="field">
      <label for="yBot">Erkennung bis unten (Bildhöhe)</label><output id="yBotOut">100 %</output>
      <input type="range" id="yBot" min="10" max="100" step="1">
    </div>
    <div class="field">
      <label for="sens">Auslöseschwelle (kleiner = empfindlicher)</label><output id="sensOut">12 %</output>
      <input type="range" id="sens" min="5" max="40" step="1">
    </div>

    <div class="btns">
      <button class="primary" id="btnCam" type="button">Kamera starten</button>
    </div>
    <p class="err" id="camErr" role="alert" hidden></p>
  </section>

  <section class="card" aria-labelledby="hMeas">
    <h2 id="hMeas">Messen</h2>
    <div class="btns two">
      <button id="btnSync" type="button" disabled>1 · Synchronisieren</button>
      <button class="primary" id="btnMeasure" type="button" disabled>2 · Messung scharf</button>
    </div>
    <p class="hint">Zuerst synchronisieren: beide Handys nebeneinander, dann beide Objektive mit einer Karte oder Hand abdecken und die Karte gerade wegziehen. Danach die Handys an Start und Ziel stellen.</p>
  </section>

  <section class="card" aria-labelledby="hRuns">
    <h2 id="hRuns">Auslösungen dieses Handys</h2>
    <ol class="runs" id="runs"></ol>
    <p class="empty" id="runsEmpty">Noch nichts erfasst.</p>
    <div class="row">
      <button class="small" id="btnClear" type="button">Liste leeren</button>
    </div>
  </section>

  <section class="card" aria-labelledby="hRes">
    <h2 id="hRes">Flying-30-Zeiten</h2>
    <label class="note" for="mine">Liste dieses Handys. Kopieren und an das andere Handy schicken, zum Beispiel per Messenger.</label>
    <textarea id="mine" readonly rows="2"></textarea>
    <div class="row"><button class="small" id="btnCopy" type="button">Liste kopieren</button><span class="note" id="copyMsg" role="status"></span></div>

    <label class="note" for="other">Liste vom anderen Handy hier einfügen</label>
    <textarea id="other" rows="2" placeholder="FLY30|Z|12.3456,31.2210"></textarea>
    <p class="err" id="resErr" role="alert" hidden></p>
    <div class="tablewrap" id="resWrap" hidden>
      <table>
        <thead><tr><th>Lauf</th><th>Zeit s</th><th>m/s</th><th>km/h</th></tr></thead>
        <tbody id="resBody"></tbody>
      </table>
    </div>
    <div class="sum" id="resSum"></div>
  </section>

  <details class="card">
    <summary>Anleitung</summary>
    <div class="body">
      <p><b>Seite öffnen.</b> Die Kamera funktioniert nur, wenn die Seite über https geöffnet wird. Auf beiden Handys dieselbe Adresse öffnen, eines auf Start, das andere auf Ziel stellen.</p>
      <ol>
        <li>Handy quer zur Bahn auf Stativ oder Kegel stellen, etwa 3 bis 5 m seitlich, Höhe Hüfte bis Brust, Querformat. Kamera ruhig und fest stehen lassen.</li>
        <li>Einen Kegel oder Stab auf die Linie stellen. Den Regler „Torposition“ so verschieben, dass das Band genau über dem Kegel liegt.</li>
        <li>Mit „Erkennung ab oben“ und „bis unten“ den Bereich einengen, bis nur die Hüfte im hellen Band liegt. Der abgedunkelte Bereich wird nicht ausgewertet, Beine und Arme außerhalb lösen dann nicht aus. Nach jeder Änderung lernt das Handy den Hintergrund neu, dabei nicht durchs Tor laufen.</li>
        <li>Im Zielbild darf hinter der Bahn nichts laufen und die Sonne nicht direkt ins Objektiv scheinen.</li>
        <li>Synchronisieren: beide Handys dicht nebeneinander, gleiche Ausrichtung, auf beiden „Synchronisieren“ tippen. Nach dem Lernen (etwa eine Sekunde) beide Objektive gemeinsam abdecken und die Karte gerade vom Handy weg abziehen. Beide Handys piepen. Dann erst zu Start und Ziel tragen.</li>
        <li>Auf beiden Handys „Messung scharf“ tippen und einen Moment stillhalten, bis „Scharf“ steht. Danach laufen. Nach jedem Lauf rearmieren die Handys selbst.</li>
        <li>Am Ende Liste auf einem Handy kopieren, auf dem anderen einfügen. Die Zeiten werden anhand der Zeitpunkte zugeordnet.</li>
      </ol>
      <p><b>Genauigkeit.</b> Die Zeit wird auf dem Bildraster gemessen, dazwischen wird interpoliert. Bei 60 Bildern pro Sekunde liegt ein einzelner Torwert typisch im Bereich von 10 bis 15 ms. Die Auswertung zeigt dir bei „Kamera“ oben die echte Bildrate. Bei nur 30 Bildern pro Sekunde ist es ungefähr doppelt so grob. Wiederhole die Synchronisierung etwa alle 15 Minuten.</p>
      <p><b>Falsche Auslösung.</b> Läuft jemand anderes durchs Tor, die Zeile in der Liste löschen und die Listen neu kopieren.</p>
    </div>
  </details>

</main>

<script>
// ===== DETECTOR START =====
// Läufer-Erkennung: Ein schmales senkrechtes Band wird mit einem Hintergrundbild verglichen.
// Ausgelöst wird, wenn der Anteil veränderter Bildpunkte die Schwelle überschreitet.
// Die Zeit zwischen zwei Bildern wird linear interpoliert.
function createDetector(n, userCfg) {
  var cfg = { pixThresh: 22, trigger: 0.12, learnSec: 0.8, lockSec: 1.5, releaseSec: 0.4, alphaLearn: 0.3, alphaRun: 0.02 };
  for (var key in (userCfg || {})) cfg[key] = userCfg[key];
  var bg = new Float32Array(n), d = new Float32Array(n), tmp = new Float32Array(n);
  var state = 'off', tState = 0, tTrig = -1e9, lowSince = null, lastE = 0, lastT = 0, haveBg = false, shift = 0;
  function updateBg(luma, a) { for (var i = 0; i < n; i++) bg[i] += a * (luma[i] - bg[i]); }
  return {
    cfg: cfg,
    get state() { return state; },
    arm: function (t) { state = 'learning'; tState = t; haveBg = false; lowSince = null; lastE = 0; shift = 0; },
    disarm: function () { state = 'off'; },
    push: function (luma, t) {
      if (state === 'off') return { energy: 0, state: state, event: null };
      if (!haveBg) { bg.set(luma); haveBg = true; lastT = t; lastE = 0; return { energy: 0, state: state, event: null }; }
      var i;
      for (i = 0; i < n; i++) d[i] = luma[i] - bg[i];
      // Belichtungsänderungen der Kamera betreffen alle Punkte gleich: Median als gemeinsame Verschiebung abziehen.
      if (lastE < cfg.trigger * 0.5) { tmp.set(d); tmp.sort(); shift = tmp[n >> 1]; }
      var cnt = 0;
      for (i = 0; i < n; i++) { var v = d[i] - shift; if (v > cfg.pixThresh || v < -cfg.pixThresh) cnt++; }
      var e = cnt / n, event = null, quiet = e < cfg.trigger * 0.5;
      if (state === 'learning') {
        updateBg(luma, cfg.alphaLearn);
        if (t - tState >= cfg.learnSec) state = 'armed';
      } else if (state === 'armed') {
        if (e >= cfg.trigger && lastE < cfg.trigger) {
          var frac = (cfg.trigger - lastE) / (e - lastE);
          event = { t: lastT + frac * (t - lastT) };
          state = 'locked'; tTrig = t; lowSince = null;
        } else if (quiet) updateBg(luma, cfg.alphaRun);
      } else if (state === 'locked') {
        if (quiet) {
          if (lowSince === null) lowSince = t;
          if (t - lowSince >= cfg.releaseSec && t - tTrig >= cfg.lockSec) state = 'armed';
          updateBg(luma, cfg.alphaRun);
        } else lowSince = null;
      }
      lastE = e; lastT = t;
      return { energy: e, state: state, event: event };
    }
  };
}

// Sync-Ereignis: plötzliche Helligkeitsänderung des ganzen Bandes (Objektiv abdecken oder freigeben).
function createStepDetector(userCfg) {
  var cfg = { delta: 14, learnSec: 0.6, alphaLearn: 0.15, alphaRun: 0.05 };
  for (var key in (userCfg || {})) cfg[key] = userCfg[key];
  var base = null, state = 'off', tStart = 0, lastD = 0, lastT = 0;
  return {
    cfg: cfg,
    get state() { return state; },
    arm: function (t) { state = 'learning'; tStart = t; base = null; lastD = 0; lastT = t; },
    disarm: function () { state = 'off'; },
    push: function (mean, t) {
      if (state === 'off' || state === 'done') return { energy: 0, state: state, event: null };
      if (base === null) { base = mean; lastT = t; lastD = 0; return { energy: 0, state: state, event: null }; }
      var d = Math.abs(mean - base), event = null;
      if (state === 'learning') {
        base += cfg.alphaLearn * (mean - base);
        if (t - tStart >= cfg.learnSec) state = 'armed';
      } else if (state === 'armed') {
        if (d >= cfg.delta && lastD < cfg.delta) {
          var frac = (cfg.delta - lastD) / (d - lastD);
          event = { t: lastT + frac * (t - lastT) };
          state = 'done';
        } else if (d < cfg.delta * 0.5) base += cfg.alphaRun * (mean - base);
      }
      lastD = d; lastT = t;
      return { energy: d / cfg.delta, state: state, event: event };
    }
  };
}

// Start- und Zielzeiten (Sekunden seit Sync) zu Läufen zusammenführen.
function pairRuns(starts, finishes, minT, maxT) {
  var S = starts.slice().sort(function (a, b) { return a - b; });
  var F = finishes.slice().sort(function (a, b) { return a - b; });
  var used = new Array(S.length).fill(false), pairs = [];
  for (var j = 0; j < F.length; j++) {
    for (var i = S.length - 1; i >= 0; i--) {
      if (used[i]) continue;
      var dt = F[j] - S[i];
      if (dt < minT) continue;
      if (dt > maxT) break;
      used[i] = true;
      pairs.push({ start: S[i], finish: F[j], time: dt });
      break;
    }
  }
  return { pairs: pairs, unmatchedStarts: S.length - pairs.length, unmatchedFinishes: F.length - pairs.length };
}

function formatList(role, times) {
  return 'FLY30|' + role + '|' + times.map(function (x) { return x.toFixed(4); }).join(',');
}
function parseList(text) {
  var m = /FLY30\|([SZ])\|([^\s|]*)/.exec(String(text || ''));
  if (!m) return null;
  var times = m[2] === '' ? [] : m[2].split(',').map(parseFloat).filter(function (x) { return isFinite(x); });
  return { role: m[1], times: times };
}
// ===== DETECTOR END =====

(function () {
  'use strict';
  var $ = function (id) { return document.getElementById(id); };
  var store = {
    get: function (k, d) { try { var v = localStorage.getItem('f30.' + k); return v === null ? d : JSON.parse(v); } catch (e) { return d; } },
    set: function (k, v) { try { localStorage.setItem('f30.' + k, JSON.stringify(v)); } catch (e) {} }
  };
  var clamp = function (x, a, b) { return Math.min(b, Math.max(a, x)); };
  var fmt = function (x, d) { return x.toFixed(d == null ? 3 : d).replace('.', ','); };

  var SW = 2, SH = 240;
  var vid = $('vid'), ov = $('ov'), spark = $('spark'), frame = $('frame');
  var sc = document.createElement('canvas'); sc.width = SW; sc.height = SH;
  var sctx = sc.getContext('2d', { willReadFrequently: true });
  sctx.imageSmoothingQuality = 'high';
  var luma = new Float32Array(SH);

  var S = {
    role: store.get('role', 'S') === 'Z' ? 'Z' : 'S',
    pos: clamp(+store.get('pos', 50) || 50, 5, 95),
    sens: clamp(+store.get('sens', 12) || 12, 5, 40),
    top: clamp(+store.get('top', 0) || 0, 0, 90),
    bot: clamp(+store.get('bot', 100) || 100, 10, 100),
    runs: (store.get('runs', []) || []).filter(function (x) { return typeof x === 'number' && isFinite(x); }),
    tRef: null, mode: 'idle', stream: null, running: false,
    fpsT: [], lastT: 0, useCapture: null, hasRvfc: typeof vid.requestVideoFrameCallback === 'function',
    wake: null, clearArm: false
  };
  if (S.bot - S.top < 5) { S.top = 0; S.bot = 100; }
  var det = createDetector(SH, { trigger: S.sens / 100 });
  var sdet = createStepDetector();
  var hist = new Float32Array(240), hi = 0;
  var lastState = '', lastHint = '';
  var colors = {};
  function readColors() {
    var cs = getComputedStyle(document.documentElement);
    ['--ok', '--warn', '--crit', '--accent', '--muted', '--line', '--ink'].forEach(function (k) { colors[k] = cs.getPropertyValue(k).trim(); });
  }
  readColors();
  if (window.matchMedia) window.matchMedia('(prefers-color-scheme: dark)').addEventListener('change', readColors);

  // ---------- Anzeige ----------
  function setRoleUI() {
    $('roleS').checked = S.role === 'S'; $('roleZ').checked = S.role === 'Z';
  }
  function renderSync() {
    var c = $('chipSync');
    if (S.tRef !== null) { c.textContent = 'synchron'; c.className = 'chip ok'; }
    else { c.textContent = 'nicht synchron'; c.className = 'chip warn'; }
  }
  function renderButtons() {
    var cam = !!S.stream;
    $('btnCam').textContent = cam ? 'Kamera stoppen' : 'Kamera starten';
    $('btnCam').classList.toggle('primary', !cam);
    $('btnSync').disabled = !cam || S.mode === 'measure';
    $('btnSync').textContent = S.mode === 'sync' ? 'Sync läuft · Abbrechen' : '1 · Synchronisieren';
    $('btnMeasure').disabled = !cam || S.tRef === null || S.mode === 'sync';
    $('btnMeasure').textContent = S.mode === 'measure' ? 'Messung stoppen' : '2 · Messung scharf';
    $('btnMeasure').classList.toggle('primary', S.mode !== 'measure');
  }
  function renderStatus() {
    var st = S.mode === 'sync' ? sdet.state : det.state;
    var key = (S.stream ? 'on' : 'off') + S.mode + st;
    if (key === lastState) return;
    lastState = key;
    var txt = 'Kamera aus', cls = '', hint = '';
    if (S.stream) {
      if (S.mode === 'sync') {
        if (st === 'learning') { txt = 'Lernt Hintergrund'; cls = 'learning'; hint = 'Beide Objektive frei lassen und nicht bewegen.'; }
        else if (st === 'armed') { txt = 'Warte auf Sync'; cls = 'armed'; hint = 'Jetzt beide Objektive gemeinsam abdecken und gerade abziehen.'; }
        else { txt = 'Sync erfasst'; cls = 'locked'; }
      } else if (S.mode === 'measure') {
        if (st === 'learning') { txt = 'Lernt Hintergrund'; cls = 'learning'; hint = 'Nicht durchs Torband laufen.'; }
        else if (st === 'armed') { txt = 'Scharf'; cls = 'armed'; hint = 'Der Lauf kann kommen.'; }
        else { txt = 'Ausgelöst'; cls = 'locked'; hint = 'Wird gleich wieder scharf.'; }
      } else {
        txt = 'Vorschau'; hint = S.tRef === null ? 'Bild ausrichten, Tor auf den Kegel legen. Dann synchronisieren.' : 'Noch nicht scharf. Tippe auf „Messung scharf“.';
      }
    }
    $('statusTxt').textContent = txt;
    $('dot').className = 'dot ' + cls;
    $('statusHint').textContent = hint;
    lastHint = hint;
  }
  function renderRuns() {
    var ol = $('runs'); ol.textContent = '';
    S.runs.forEach(function (t, i) {
      var li = document.createElement('li');
      var n = document.createElement('span'); n.className = 'n'; n.textContent = '#' + (i + 1);
      var v = document.createElement('span'); v.className = 'v'; v.textContent = fmt(t) + ' s';
      var b = document.createElement('button'); b.type = 'button'; b.textContent = 'Löschen';
      b.setAttribute('aria-label', 'Auslösung ' + (i + 1) + ' löschen');
      b.addEventListener('click', function () { S.runs.splice(i, 1); persistRuns(); renderAll(); });
      li.appendChild(n); li.appendChild(v); li.appendChild(b); ol.appendChild(li);
    });
    $('runsEmpty').hidden = S.runs.length > 0;
    $('btnClear').disabled = S.runs.length === 0;
    $('mine').value = S.runs.length ? formatList(S.role, S.runs) : '';
  }
  function renderResults() {
    var err = $('resErr'), wrap = $('resWrap'), sum = $('resSum');
    err.hidden = true; wrap.hidden = true; sum.textContent = '';
    var txt = $('other').value.trim();
    if (!txt) return;
    var o = parseList(txt);
    if (!o) { err.textContent = 'Das sieht nicht nach einer Liste aus. Sie beginnt mit FLY30.'; err.hidden = false; return; }
    if (o.role === S.role) { err.textContent = 'Beide Listen sind vom ' + (S.role === 'S' ? 'Start' : 'Ziel') + '. Eine Liste muss vom anderen Handy stammen.'; err.hidden = false; return; }
    var starts = S.role === 'S' ? S.runs : o.times, fins = S.role === 'Z' ? S.runs : o.times;
    var r = pairRuns(starts, fins, 2.0, 9.0);
    var body = $('resBody'); body.textContent = '';
    if (!r.pairs.length) { err.textContent = 'Keine passenden Läufe gefunden. Start und Ziel müssen zwischen 2 und 9 s auseinanderliegen und denselben Sync haben.'; err.hidden = false; return; }
    var best = Infinity; r.pairs.forEach(function (p) { if (p.time < best) best = p.time; });
    var total = 0;
    r.pairs.forEach(function (p, i) {
      var tr = document.createElement('tr'); if (p.time === best) tr.className = 'best';
      var cells = [String(i + 1), fmt(p.time), fmt(30 / p.time, 2), fmt(30 / p.time * 3.6, 1)];
      cells.forEach(function (c, k) { var td = document.createElement('td'); if (k === 1) td.className = 't'; td.textContent = c; tr.appendChild(td); });
      body.appendChild(tr); total += p.time;
    });
    wrap.hidden = false;
    var parts = [['Bestzeit', fmt(best) + ' s'], ['Schnitt', fmt(total / r.pairs.length) + ' s']];
    parts.forEach(function (p) { var s = document.createElement('span'); s.textContent = p[0] + ' '; var b = document.createElement('b'); b.textContent = p[1]; s.appendChild(b); sum.appendChild(s); });
    if (r.unmatchedStarts || r.unmatchedFinishes) {
      var s2 = document.createElement('span'); s2.className = 'note';
      s2.textContent = 'Ohne Partner: ' + r.unmatchedStarts + ' Start, ' + r.unmatchedFinishes + ' Ziel';
      sum.appendChild(s2);
    }
  }
  function renderAll() { setRoleUI(); renderSync(); renderButtons(); renderRuns(); renderResults(); lastState = ''; renderStatus(); }
  function persistRuns() { store.set('runs', S.runs); }

  // ---------- Overlay und Verlauf ----------
  function sizeCanvas(c) {
    var dpr = window.devicePixelRatio || 1, w = c.clientWidth, h = c.clientHeight;
    if (c.width !== Math.round(w * dpr) || c.height !== Math.round(h * dpr)) { c.width = Math.round(w * dpr); c.height = Math.round(h * dpr); }
    return dpr;
  }
  function drawOverlay() {
    var dpr = sizeCanvas(ov), ctx = ov.getContext('2d'), w = ov.width, h = ov.height;
    ctx.clearRect(0, 0, w, h);
    var st = S.mode === 'sync' ? sdet.state : det.state;
    var col = st === 'armed' ? colors['--ok'] : st === 'learning' ? colors['--warn'] : (st === 'locked' || st === 'done') ? colors['--crit'] : colors['--accent'];
    var vw = vid.videoWidth || 1280;
    var bw = Math.max(3 * dpr, Math.max(4, Math.round(vw * 0.005)) / vw * w);
    var x = S.pos / 100 * w, y0 = S.top / 100 * h, y1 = S.bot / 100 * h;
    ctx.globalAlpha = 0.55; ctx.fillStyle = '#000';
    ctx.fillRect(0, 0, w, y0); ctx.fillRect(0, y1, w, h - y1);
    ctx.globalAlpha = 0.35; ctx.fillStyle = col; ctx.fillRect(x - bw / 2, y0, bw, y1 - y0);
    ctx.globalAlpha = 1; ctx.fillRect(x - 1 * dpr, y0, 2 * dpr, y1 - y0);
    ctx.strokeStyle = col; ctx.lineWidth = 1 * dpr; ctx.setLineDash([6 * dpr, 5 * dpr]);
    if (S.top > 0) { ctx.beginPath(); ctx.moveTo(0, y0); ctx.lineTo(w, y0); ctx.stroke(); }
    if (S.bot < 100) { ctx.beginPath(); ctx.moveTo(0, y1); ctx.lineTo(w, y1); ctx.stroke(); }
    ctx.setLineDash([]);
    ctx.font = '700 ' + Math.round(14 * dpr) + 'px ' + getComputedStyle(document.body).getPropertyValue('--display');
    ctx.textBaseline = 'top';
    var label = S.role === 'S' ? 'START' : 'ZIEL';
    var tw = ctx.measureText(label).width, lx = x + 8 * dpr + tw > w ? x - 8 * dpr - tw : x + 8 * dpr;
    ctx.fillText(label, lx, y0 + 6 * dpr);
  }
  function drawSpark() {
    var dpr = sizeCanvas(spark), ctx = spark.getContext('2d'), w = spark.width, h = spark.height;
    ctx.clearRect(0, 0, w, h);
    var yThr = h * 0.5;
    ctx.strokeStyle = colors['--accent']; ctx.lineWidth = 1 * dpr; ctx.setLineDash([4 * dpr, 4 * dpr]);
    ctx.beginPath(); ctx.moveTo(0, yThr); ctx.lineTo(w, yThr); ctx.stroke(); ctx.setLineDash([]);
    ctx.strokeStyle = colors['--ink']; ctx.lineWidth = 1.5 * dpr; ctx.beginPath();
    var N = hist.length;
    for (var i = 0; i < N; i++) {
      var v = clamp(hist[(hi + i) % N], 0, 2) / 2;
      var x = i / (N - 1) * w, y = h - 2 * dpr - v * (h - 4 * dpr);
      if (i === 0) ctx.moveTo(x, y); else ctx.lineTo(x, y);
    }
    ctx.stroke();
  }

  // ---------- Signale ----------
  var ac = null;
  function unlockAudio() { try { if (!ac) ac = new (window.AudioContext || window.webkitAudioContext)(); if (ac.state === 'suspended') ac.resume(); } catch (e) {} }
  function beep() {
    try {
      if (ac) { var o = ac.createOscillator(), g = ac.createGain(); o.frequency.value = 1320; g.gain.value = 0.25; o.connect(g); g.connect(ac.destination); o.start(); o.stop(ac.currentTime + 0.09); }
    } catch (e) {}
    try { if (navigator.vibrate) navigator.vibrate(60); } catch (e) {}
    frame.classList.add('hit'); setTimeout(function () { frame.classList.remove('hit'); }, 350);
  }

  // ---------- Bildschleife ----------
  function onFrame(now, meta) {
    if (!S.running) return;
    if (S.useCapture === null) S.useCapture = !!(meta && typeof meta.captureTime === 'number');
    var tms = S.useCapture ? meta.captureTime : now;
    process(tms / 1000);
    schedule();
  }
  function schedule() {
    if (S.hasRvfc) vid.requestVideoFrameCallback(onFrame);
    else requestAnimationFrame(function (ts) { onFrame(ts, null); });
  }
  function process(t) {
    var vw = vid.videoWidth, vh = vid.videoHeight;
    if (!vw || !vh) return;
    S.lastT = t;
    var sw = Math.max(4, Math.round(vw * 0.005));
    var sx = clamp(Math.round(S.pos / 100 * vw - sw / 2), 0, vw - sw);
    var sy = clamp(Math.round(S.top / 100 * vh), 0, vh - 8);
    var sh = clamp(Math.round((S.bot - S.top) / 100 * vh), 8, vh - sy);
    sctx.drawImage(vid, sx, sy, sw, sh, 0, 0, SW, SH);
    var px = sctx.getImageData(0, 0, SW, SH).data, sum = 0;
    for (var y = 0; y < SH; y++) {
      var s = 0;
      for (var x = 0; x < SW; x++) { var i = (y * SW + x) * 4; s += 0.299 * px[i] + 0.587 * px[i + 1] + 0.114 * px[i + 2]; }
      luma[y] = s / SW; sum += luma[y];
    }
    var r;
    if (S.mode === 'sync') r = sdet.push(sum / SH, t);
    else r = det.push(luma, t);
    var norm = S.mode === 'sync' ? r.energy : r.energy / det.cfg.trigger;
    hist[hi] = norm; hi = (hi + 1) % hist.length;
    if (r.event) onEvent(r.event.t);
    S.fpsT.push(t); if (S.fpsT.length > 40) S.fpsT.shift();
    if (S.fpsT.length > 10 && (S.fpsT.length % 10 === 0)) {
      var fps = (S.fpsT.length - 1) / (S.fpsT[S.fpsT.length - 1] - S.fpsT[0]);
      var c = $('chipFps'); c.textContent = Math.round(fps) + ' Bilder/s'; c.className = 'chip ' + (fps >= 50 ? 'ok' : 'warn');
    }
    renderStatus(); drawOverlay(); drawSpark();
  }
  function onEvent(t) {
    if (S.mode === 'sync') {
      S.tRef = t; S.runs = []; persistRuns(); S.mode = 'idle'; sdet.disarm(); det.arm(S.lastT);
      beep(); renderAll();
    } else if (S.mode === 'measure') {
      if (S.tRef === null) return;
      S.runs.push(t - S.tRef); persistRuns(); beep(); renderRuns(); renderResults();
    }
  }

  // ---------- Kamera ----------
  function showErr(msg) { var e = $('camErr'); e.textContent = msg || ''; e.hidden = !msg; }
  async function startCam() {
    showErr('');
    if (!navigator.mediaDevices || !navigator.mediaDevices.getUserMedia) {
      showErr('Kamerazugriff ist hier nicht möglich. Öffne die Seite über eine https-Adresse, nicht als Datei oder in einer Vorschau.');
      return;
    }
    try {
      var stream = await navigator.mediaDevices.getUserMedia({
        audio: false,
        video: { facingMode: { ideal: 'environment' }, width: { ideal: 1280 }, height: { ideal: 720 }, frameRate: { ideal: 60 } }
      });
      S.stream = stream; vid.srcObject = stream;
      await vid.play().catch(function () {});
      await new Promise(function (res) { if (vid.videoWidth) res(); else vid.addEventListener('loadedmetadata', res, { once: true }); });
      frame.style.setProperty('--ar', (vid.videoWidth / vid.videoHeight).toFixed(4));
      $('ph').hidden = true;
      S.running = true; S.fpsT = []; S.useCapture = null;
      det.arm(performance.now() / 1000);
      schedule();
      try { if (navigator.wakeLock) S.wake = await navigator.wakeLock.request('screen'); } catch (e) {}
      var set = stream.getVideoTracks()[0].getSettings ? stream.getVideoTracks()[0].getSettings() : {};
      if (set.frameRate) { var c = $('chipFps'); c.textContent = Math.round(set.frameRate) + ' Bilder/s'; }
      if (!S.hasRvfc) showErr('Dieser Browser liefert keine genauen Bildzeiten. Die Messung wird gröber, etwa ±20 ms. Chrome oder Safari ab Version 15.4 nutzen.');
    } catch (e) {
      stopCam();
      showErr(e && e.name === 'NotAllowedError' ? 'Kamerazugriff wurde abgelehnt. Erlaube die Kamera in den Browser-Einstellungen für diese Seite.' : 'Kamera konnte nicht gestartet werden: ' + (e && e.message ? e.message : e));
    }
  }
  function stopCam() {
    S.running = false; S.mode = 'idle'; det.disarm(); sdet.disarm();
    if (S.stream) S.stream.getTracks().forEach(function (t) { t.stop(); });
    S.stream = null; vid.srcObject = null; $('ph').hidden = false;
    try { if (S.wake) S.wake.release(); } catch (e) {} S.wake = null;
    var c = $('chipFps'); c.textContent = 'Kamera aus'; c.className = 'chip';
    hist.fill(0); renderAll(); drawOverlay(); drawSpark();
  }

  // ---------- Bedienung ----------
  $('btnCam').addEventListener('click', function () { unlockAudio(); if (S.stream) stopCam(); else startCam().then(renderAll); });
  $('btnSync').addEventListener('click', function () {
    unlockAudio();
    if (S.mode === 'sync') { S.mode = 'idle'; sdet.disarm(); det.arm(S.lastT); }
    else { S.mode = 'sync'; det.disarm(); sdet.arm(S.lastT); }
    renderAll();
  });
  $('btnMeasure').addEventListener('click', function () {
    unlockAudio();
    if (S.mode === 'measure') S.mode = 'idle'; else S.mode = 'measure';
    det.arm(S.lastT); renderAll();
  });
  $('btnClear').addEventListener('click', function () {
    var b = $('btnClear');
    if (!S.clearArm) { S.clearArm = true; b.textContent = 'Wirklich leeren?'; setTimeout(function () { S.clearArm = false; b.textContent = 'Liste leeren'; }, 3000); return; }
    S.clearArm = false; b.textContent = 'Liste leeren'; S.runs = []; persistRuns(); renderAll();
  });
  document.querySelectorAll('input[name=role]').forEach(function (r) {
    r.addEventListener('change', function () { if (r.checked) { S.role = r.value; store.set('role', S.role); renderAll(); drawOverlay(); } });
  });
  $('pos').value = S.pos; $('posOut').textContent = S.pos + ' %';
  $('pos').addEventListener('input', function () {
    S.pos = +this.value; $('posOut').textContent = S.pos + ' %'; store.set('pos', S.pos);
    if (S.running && S.mode !== 'sync') det.arm(S.lastT);
    drawOverlay();
  });
  $('sens').value = S.sens; $('sensOut').textContent = S.sens + ' %';
  $('sens').addEventListener('input', function () {
    S.sens = +this.value; $('sensOut').textContent = S.sens + ' %'; store.set('sens', S.sens);
    det.cfg.trigger = S.sens / 100;
  });
  function bandUI() {
    $('yTop').value = S.top; $('yBot').value = S.bot;
    $('yTopOut').textContent = S.top + ' %'; $('yBotOut').textContent = S.bot + ' %';
  }
  bandUI();
  function bandChange(which) {
    S.top = +$('yTop').value; S.bot = +$('yBot').value;
    if (S.bot - S.top < 5) { if (which === 'top') S.top = S.bot - 5; else S.bot = S.top + 5; }
    store.set('top', S.top); store.set('bot', S.bot); bandUI();
    if (S.running && S.mode !== 'sync') det.arm(S.lastT);
    drawOverlay();
  }
  $('yTop').addEventListener('input', function () { bandChange('top'); });
  $('yBot').addEventListener('input', function () { bandChange('bot'); });
  $('btnCopy').addEventListener('click', function () {
    var ta = $('mine'), msg = $('copyMsg');
    if (!ta.value) { msg.textContent = 'Noch nichts zu kopieren.'; return; }
    var done = function () { msg.textContent = 'Kopiert.'; setTimeout(function () { msg.textContent = ''; }, 2000); };
    var fallback = function () { ta.focus(); ta.select(); try { document.execCommand('copy') ? done() : (msg.textContent = 'Text markiert, bitte manuell kopieren.'); } catch (e) { msg.textContent = 'Text markiert, bitte manuell kopieren.'; } };
    if (navigator.clipboard && navigator.clipboard.writeText) navigator.clipboard.writeText(ta.value).then(done, fallback); else fallback();
  });
  $('other').addEventListener('input', renderResults);
  window.addEventListener('resize', function () { drawOverlay(); drawSpark(); });
  document.addEventListener('visibilitychange', async function () {
    if (document.visibilityState === 'visible' && S.stream && navigator.wakeLock) { try { S.wake = await navigator.wakeLock.request('screen'); } catch (e) {} }
  });

  renderAll(); drawOverlay(); drawSpark();
})();
</script>
</body>
</html>
