---
title: Poster Voting
layout: sub
permalink: /poster-voting/
---

<html lang="en">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width,initial-scale=1" />
<title>2025 IEEE CyberSciTech/DASC/PICom/CBDCom – Voting for the Best Poster</title>
<style>
  html, body { font-family: system-ui, -apple-system, "Segoe UI", Roboto, "Helvetica Neue", Arial; margin:0; padding:0; background:#fff; color:#111; }
  .card { max-width: 800px; margin: 0 auto; padding: 20px 16px; }
  h2 { font-size: 1.25rem; line-height: 1.4; margin-bottom: 8px; }
  .muted { color:#555; font-size:0.95rem; line-height:1.5; }
  .hidden { display:none; }
  .btn { display:inline-block; padding:10px 18px; border:1px solid #ccc; border-radius:8px; background:#f8f8f8; cursor:pointer; font-size:0.95rem; }
  .btn:hover { background:#eee; }
  .toolbar { display:flex; justify-content:flex-end; gap:8px; margin:8px 0; }
  iframe { width:100%; height:100dvh; border:0; margin-top:12px; }
  @media (max-width: 600px) {
    .card { max-width:none; padding:0; }
    h2, .muted { padding: 12px 12px 0; }
  }
</style>
<script>
  // === Config ===
  const FORM_ID = '1FAIpQLSdhRpCrafvTMqVIfcXWNh_ZoL0XwLIxcUqnzFwx-ySSpH6vhQ';
  const IFRAME_SRC_EMBED = `https://docs.google.com/forms/d/e/${FORM_ID}/viewform?embedded=true`;
  const IFRAME_SRC_FULLSCREEN = `https://docs.google.com/forms/d/e/${FORM_ID}/viewform`;
  const ORGANIZER_PIN = 'CST25-Poster'; // ← Set your PIN here

  // === Persistence (Cookie + LocalStorage) ===
  function setSubmittedFlag(days = 365) {
    try { localStorage.setItem(`form_${FORM_ID}_submitted`, '1'); } catch {}
    const d = new Date(); d.setTime(d.getTime() + days*24*60*60*1000);
    document.cookie = `form_${FORM_ID}_submitted=1; expires=${d.toUTCString()}; path=/; SameSite=Lax`;
  }
  function hasSubmittedFlag() {
    try { if (localStorage.getItem(`form_${FORM_ID}_submitted`) === '1') return true; } catch {}
    return document.cookie.split('; ').some(row => row.startsWith(`form_${FORM_ID}_submitted=`));
  }
  function clearLocalMarker() {
    try { localStorage.removeItem(`form_${FORM_ID}_submitted`); } catch {}
    document.cookie = `form_${FORM_ID}_submitted=; expires=Thu, 01 Jan 1970 00:00:00 UTC; path=/; SameSite=Lax`;
  }

  // === Organizer unlock on THIS device ===
  function organizerUnlock() {
    const pin = prompt('Organizer PIN required to allow re-submission on THIS device:');
    if (pin !== ORGANIZER_PIN) {
      alert('Invalid PIN.');
      return;
    }
    clearLocalMarker();
    alert('Re-submission enabled on this device. The form will reload.');
    location.reload();
  }

  document.addEventListener('DOMContentLoaded', () => {
    const container = document.getElementById('container');
    const done = document.getElementById('done');
    const denied = document.getElementById('denied');

    if (hasSubmittedFlag()) {
      container.classList.add('hidden');
      denied.classList.remove('hidden');
      return;
    }

    let loadCount = 0;
    const iframe = document.createElement('iframe');
    iframe.id = 'gform';
    iframe.src = IFRAME_SRC_EMBED;

    iframe.addEventListener('load', () => {
      loadCount++;
      // 1st load = viewform; 2nd load = formResponse after submission
      if (loadCount >= 2) {
        setSubmittedFlag();
        container.classList.add('hidden');
        done.classList.remove('hidden');
      }
    });

    document.getElementById('frameHost').appendChild(iframe);

    // Manual fallback if auto-detection fails
    document.getElementById('markSubmitted').addEventListener('click', () => {
      setSubmittedFlag();
      container.classList.add('hidden');
      done.classList.remove('hidden');
    });

    // Fullscreen link
    const openFull = document.getElementById('openFull');
    openFull.href = IFRAME_SRC_FULLSCREEN;
  });
</script>
</head>
<body>
  <!-- Main form container -->
  <div class="card" id="container">
    <h2>2025 IEEE CyberSciTech/DASC/PICom/CBDCom – Voting for the Best Poster</h2>
    <div class="toolbar">
      <a id="openFull" class="btn" target="_blank" rel="noopener">Open full screen</a>
    </div>

    <div id="frameHost"></div>

    <p class="muted" style="margin:12px 0;">
      If your submission completes but the page does not auto-detect it, click:
      <button id="markSubmitted" class="btn">I have submitted</button>
    </p>
  </div>

  <!-- Done screen (after successful submission detected) -->
  <div class="card hidden" id="done">
    <h2>Thank you for your submission!</h2>
    <p class="muted">Your vote has been recorded on this device.</p>
    <p class="muted" style="margin-top:8px;">
      <strong>Need to correct your vote?</strong>
      Please tap the button below and ask an organizer to enter the PIN to enable re-submission on <em>this device only</em>.
    </p>
    <button class="btn" onclick="organizerUnlock()">Re-vote (Organizer PIN required)</button>
    <div style="margin-top:8px;">
      <button class="btn" onclick="location.reload()">Refresh</button>
    </div>
  </div>

<div class="card hidden" id="denied">
  <h2>Already submitted on this device</h2>
  <p class="muted">
    If you need to re-vote due to an error, please ask an organizer to enable re-submission on
    <em>this device only</em>.
  </p>
  <div style="margin-top:8px; display:flex; gap:8px; flex-wrap:wrap;">
    <button class="btn" onclick="organizerUnlock()">Re-vote (Organizer PIN required)</button>
    <button class="btn" onclick="location.reload()">Refresh</button>
  </div>
</div>
</body>
</html>
