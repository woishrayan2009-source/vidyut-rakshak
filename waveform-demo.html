/* ==========================================================================
   Vidyut Rakshak — Live Waveform Demo
   Simulated oscilloscope of circuit current with three states:
     normal    → clean sine wave, teal
     overload  → amplitude ramps up smoothly, amber, caution but NOT tripped
     arcing    → erratic/spiky noise, red, trips the circuit immediately
     tripped   → flat zero line, breaker open, held until Reset
   ========================================================================== */
(function () {
  'use strict';

  var canvas = document.getElementById('waveform-canvas');
  var ctx = canvas.getContext('2d');

  var statusBanner = document.getElementById('status-banner');
  var statusText = document.getElementById('status-text');
  var statusDetail = document.getElementById('status-detail');
  var chartNote = document.getElementById('chart-note');

  var wire1 = document.getElementById('wire-1');
  var wire2 = document.getElementById('wire-2');
  var breakerBody = document.getElementById('breaker-body');
  var breakerLever = document.getElementById('breaker-lever');
  var circuitBulb = document.getElementById('circuit-bulb');
  var circuitIndicator = document.getElementById('circuit-indicator');
  var circuitStatusValue = document.getElementById('circuit-status-value');

  var logList = document.getElementById('log-list');

  var overlay = document.getElementById('alert-overlay');
  var alertAck = document.getElementById('alert-ack');

  var btnNormal = document.getElementById('btn-normal');
  var btnOverload = document.getElementById('btn-overload');
  var btnArc = document.getElementById('btn-arc');
  var btnReset = document.getElementById('btn-reset');

  // ---------------------------------------------------------------------
  // Waveform state
  // ---------------------------------------------------------------------
  var STATE = {
    NORMAL: 'normal',
    OVERLOAD: 'overload',
    ARCING: 'arcing',
    TRIPPED: 'tripped'
  };

  var state = STATE.NORMAL;
  var phase = 0;               // running sine phase
  var amplitude = 34;          // current drawn amplitude (px)
  var targetAmplitude = 34;    // amplitude the ramp is easing toward
  var arcStartedAt = null;     // timestamp when arcing began
  var buffer = [];             // scrolling sample buffer
  var bufferLen = 260;

  var COLORS = {};

  function readColors() {
    var cs = getComputedStyle(document.documentElement);
    COLORS.teal = cs.getPropertyValue('--teal').trim() || '#35e0c4';
    COLORS.amber = cs.getPropertyValue('--amber').trim() || '#ff9d2e';
    COLORS.red = cs.getPropertyValue('--red').trim() || '#ff4757';
    COLORS.grid = cs.getPropertyValue('--border-soft').trim() || '#1c2226';
    COLORS.faint = cs.getPropertyValue('--text-faint').trim() || '#565f65';
  }

  function seedBuffer() {
    buffer = [];
    for (var i = 0; i < bufferLen; i++) buffer.push(0);
  }

  // ---------------------------------------------------------------------
  // Canvas sizing (device-pixel-ratio aware)
  // ---------------------------------------------------------------------
  function resizeCanvas() {
    var wrap = canvas.parentElement;
    var rect = wrap.getBoundingClientRect();
    var dpr = window.devicePixelRatio || 1;
    canvas.width = Math.max(1, Math.floor(rect.width * dpr));
    canvas.height = Math.max(1, Math.floor(rect.height * dpr));
    ctx.setTransform(dpr, 0, 0, dpr, 0, 0);
  }

  // ---------------------------------------------------------------------
  // Sample generation per state
  // ---------------------------------------------------------------------
  function nextSample(dt) {
    var freq = 0.12; // cycles per frame-ish unit, purely visual
    phase += dt * freq;

    if (state === STATE.TRIPPED) {
      // Circuit is open — flat zero line, tiny sensor noise only.
      return (Math.random() - 0.5) * 1.2;
    }

    if (state === STATE.ARCING) {
      // Erratic: base sine with heavy noise and random spikes.
      var base = Math.sin(phase) * (amplitude * 0.5);
      var noise = (Math.random() - 0.5) * amplitude * 1.6;
      var spike = Math.random() < 0.12 ? (Math.random() - 0.5) * amplitude * 2.4 : 0;
      return base + noise + spike;
    }

    // Normal + Overload: clean sine, amplitude eased toward target.
    amplitude += (targetAmplitude - amplitude) * Math.min(1, dt * 3);
    var wobble = (Math.random() - 0.5) * (amplitude * 0.04);
    return Math.sin(phase) * amplitude + wobble;
  }

  function pushSample(v) {
    buffer.push(v);
    if (buffer.length > bufferLen) buffer.shift();
  }

  // ---------------------------------------------------------------------
  // Drawing
  // ---------------------------------------------------------------------
  function draw() {
    var rect = canvas.parentElement.getBoundingClientRect();
    var w = rect.width;
    var h = rect.height;

    ctx.clearRect(0, 0, w, h);

    // Grid
    ctx.strokeStyle = COLORS.grid;
    ctx.lineWidth = 1;
    var rows = 4;
    for (var r = 1; r < rows; r++) {
      var y = (h / rows) * r;
      ctx.beginPath();
      ctx.moveTo(0, y);
      ctx.lineTo(w, y);
      ctx.stroke();
    }

    // Center reference line
    ctx.strokeStyle = COLORS.faint;
    ctx.globalAlpha = 0.35;
    ctx.beginPath();
    ctx.moveTo(0, h / 2);
    ctx.lineTo(w, h / 2);
    ctx.stroke();
    ctx.globalAlpha = 1;

    // Trace color by state
    var strokeColor = COLORS.teal;
    if (state === STATE.OVERLOAD) strokeColor = COLORS.amber;
    if (state === STATE.ARCING) strokeColor = COLORS.red;
    if (state === STATE.TRIPPED) strokeColor = COLORS.faint;

    ctx.strokeStyle = strokeColor;
    ctx.lineWidth = 2;
    ctx.shadowColor = strokeColor;
    ctx.shadowBlur = state === STATE.TRIPPED ? 0 : 6;
    ctx.beginPath();

    var stepX = w / (bufferLen - 1);
    var midY = h / 2;
    for (var i = 0; i < buffer.length; i++) {
      var x = i * stepX;
      var y = midY - buffer[i];
      if (i === 0) ctx.moveTo(x, y);
      else ctx.lineTo(x, y);
    }
    ctx.stroke();
    ctx.shadowBlur = 0;
  }

  // ---------------------------------------------------------------------
  // Animation loop
  // ---------------------------------------------------------------------
  var lastTs = null;
  function tick(ts) {
    if (lastTs === null) lastTs = ts;
    var dt = Math.min(4, (ts - lastTs) / 16.67); // normalize to ~frame units
    lastTs = ts;

    var samplesPerFrame = state === STATE.ARCING ? 2 : 1;
    for (var s = 0; s < samplesPerFrame; s++) {
      pushSample(nextSample(dt));
    }

    draw();

    // Arc fault auto-trips after a short erratic burst.
    if (state === STATE.ARCING && arcStartedAt !== null && (ts - arcStartedAt) > 650) {
      trip();
    }

    requestAnimationFrame(tick);
  }

  // ---------------------------------------------------------------------
  // Logging
  // ---------------------------------------------------------------------
  function timeNow() {
    var d = new Date();
    function pad(n) { return n < 10 ? '0' + n : '' + n; }
    return pad(d.getHours()) + ':' + pad(d.getMinutes()) + ':' + pad(d.getSeconds());
  }

  function log(message, kind) {
    var empty = logList.querySelector('.log-empty');
    if (empty) empty.remove();
    var li = document.createElement('li');
    li.className = 'log-' + (kind || 'info');
    var time = document.createElement('span');
    time.className = 'log-time';
    time.textContent = timeNow();
    var text = document.createElement('span');
    text.textContent = message;
    li.appendChild(time);
    li.appendChild(text);
    logList.insertBefore(li, logList.firstChild);
  }

  // ---------------------------------------------------------------------
  // Circuit diagram + status banner rendering
  // ---------------------------------------------------------------------
  function setCircuitOn() {
    wire1.classList.remove('is-cut');
    wire1.classList.add('is-live');
    wire2.classList.remove('is-cut');
    wire2.classList.add('is-live');
    breakerBody.classList.remove('is-tripped');
    breakerLever.classList.remove('is-tripped');
    circuitBulb.classList.add('is-on');
    circuitIndicator.classList.remove('is-off');
    circuitIndicator.classList.add('is-on');
    circuitStatusValue.textContent = 'ON';
    circuitStatusValue.classList.remove('is-off');
    circuitStatusValue.classList.add('is-on');
  }

  function setCircuitOff() {
    wire1.classList.remove('is-live');
    wire1.classList.add('is-cut');
    wire2.classList.remove('is-live');
    wire2.classList.add('is-cut');
    breakerBody.classList.add('is-tripped');
    breakerLever.classList.add('is-tripped');
    circuitBulb.classList.remove('is-on');
    circuitIndicator.classList.remove('is-on');
    circuitIndicator.classList.add('is-off');
    circuitStatusValue.textContent = 'OFF';
    circuitStatusValue.classList.remove('is-on');
    circuitStatusValue.classList.add('is-off');
  }

  function setBanner(mode, valueText, detailText) {
    statusBanner.classList.remove('status-normal', 'status-warning', 'status-emergency');
    statusBanner.classList.add('status-' + mode);
    statusText.textContent = valueText;
    statusDetail.textContent = detailText;
  }

  function setButtonsEnabled(enabled) {
    btnNormal.disabled = !enabled;
    btnOverload.disabled = !enabled;
    btnArc.disabled = !enabled;
  }

  // ---------------------------------------------------------------------
  // State transitions
  // ---------------------------------------------------------------------
  function goNormal() {
    state = STATE.NORMAL;
    targetAmplitude = 34;
    arcStartedAt = null;
    chartNote.textContent = 'Simulated · Normal Load';
    setBanner('normal', 'NORMAL', 'Clean sinusoidal current — no fault signature detected');
    setCircuitOn();
    setButtonsEnabled(true);
    log('Normal load restored — current within safe range', 'normal');
  }

  function goOverload() {
    if (state === STATE.TRIPPED) return;
    state = STATE.OVERLOAD;
    targetAmplitude = 78;
    chartNote.textContent = 'Simulated · Overload (Caution)';
    setBanner('warning', 'CAUTION — OVERLOAD', 'Elevated current draw — no arc signature, monitoring, not tripped');
    setCircuitOn();
    log('Overload simulated — current elevated, circuit remains closed', 'warning');
  }

  function goArcing() {
    if (state === STATE.TRIPPED) return;
    state = STATE.ARCING;
    arcStartedAt = performance.now();
    chartNote.textContent = 'Simulated · Arc Fault';
    setBanner('emergency', 'FAULT DETECTED', 'Erratic arcing signature identified on monitored circuit');
    log('Arc fault signature detected — evaluating trip condition', 'emergency');
  }

  function trip() {
    state = STATE.TRIPPED;
    targetAmplitude = 0;
    amplitude = 0;
    chartNote.textContent = 'Simulated · Circuit Tripped';
    setBanner('emergency', 'ARC DETECTED — CIRCUIT TRIPPED', 'Power cut before ignition threshold — reset to resume monitoring');
    setCircuitOff();
    setButtonsEnabled(false);
    log('ARC FAULT CONFIRMED — circuit tripped, power cut to load', 'emergency');
    showAlert();
  }

  function reset() {
    hideAlert();
    seedBuffer();
    goNormal();
  }

  // ---------------------------------------------------------------------
  // Alert overlay
  // ---------------------------------------------------------------------
  function showAlert() {
    overlay.classList.add('is-visible');
    alertAck.focus();
  }
  function hideAlert() {
    overlay.classList.remove('is-visible');
  }

  // ---------------------------------------------------------------------
  // Wire up controls
  // ---------------------------------------------------------------------
  btnNormal.addEventListener('click', goNormal);
  btnOverload.addEventListener('click', goOverload);
  btnArc.addEventListener('click', goArcing);
  btnReset.addEventListener('click', reset);
  alertAck.addEventListener('click', hideAlert);
  overlay.addEventListener('click', function (e) {
    if (e.target === overlay) hideAlert();
  });
  document.addEventListener('keydown', function (e) {
    if (e.key === 'Escape' && overlay.classList.contains('is-visible')) hideAlert();
  });

  window.addEventListener('resize', resizeCanvas);

  // ---------------------------------------------------------------------
  // Init
  // ---------------------------------------------------------------------
  readColors();
  seedBuffer();
  resizeCanvas();
  setCircuitOn();
  requestAnimationFrame(tick);
})();
