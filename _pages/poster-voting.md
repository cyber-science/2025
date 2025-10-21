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
  .toolbar { display:flex; justify-content:flex-end; gap:8px; margin:8px 0; flex-wrap:wrap; }
  .pin-box { display:flex; gap:8px; align-items:center; flex-wrap:wrap; margin-top:8px; }
  .pin-input { padding:10px 12px; border:1px solid #ccc; border-radius:8px; font-size:1rem; min-width: 240px; }
  .msg { margin-top:8px; font-size:0.95rem; }
  iframe { width:100%; height:100dvh; border:0; margin-top:12px; }
  @media (max-width: 600px) {
    .card { max-width:none; padding:0; }
    h2, .muted { padding: 12px 12px 0; }
    .toolbar { padding: 0 12px; }
  }
</style>
<script>
  // === Config ===
  const FORM_ID = '1FAIpQLSdhRpCrafvTMqVIfcXWNh_ZoL0XwLIxcUqnzFwx-ySSpH6vhQ';
  const IFRAME_SRC_EMBED = `https://docs.google.com/forms/d/e/${FORM_ID}/viewform?embedded=true`;
  const IFRAME_SRC_FULLSCREEN = `https://docs.google.com/forms/d/e/${FORM_ID}/viewform`;

  // Allowed PIN hashes (SHA-256, hex). Here for PIN: CSTCPOSTERVOTE
  const ALLOWED_PIN_HASHES = [
    'bcaadde71a7434dc6d9532797ad5686d2404e019eae0c9632923d1a1b4ddd1d7'
  ];

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

  // === Hash helper (SHA-256 → hex) ===
  async function sha256Hex(str) {
    const enc = new TextEncoder();
    const data = enc.encode(str);
    const hashBuffer = await crypto.subtle.digest('SHA-256', data);
    const hashArray = Array.from(new Uint8Array(hashBuffer));
    return hashArray.map(b => b.toString(16).padStart(2, '0')).join('');
  }

  // === Verify PIN from password field and unlock on THIS device ===
  async function verifyPinAndUnlock(inputId, feedbackId) {
    const input = document.getElementById(inputId);
    const feedback = document.getElementById(feedbackId);
    const val = (input.value || '').trim();
    if (!val) {
      feedback.textContent = 'Please enter the organizer PIN.';
      feedback.style.color = '#b00020';
      return;
    }
    try {
      const h = await sha256Hex(val);
      if (ALLOWED_PIN_HASHES.includes(h)) {
        clearLocalMarker();
        feedback.textContent = 'Re-submission enabled on this device. The form will reload.';
        feedback.style.color = '#0a7f2e';
        setTimeout(() => location.reload(), 600);
      } else {
        feedback.textContent = 'Invalid PIN.';
        feedback.style.color = '#b00020';
      }
    } catch (e) {
      feedback.textContent = 'Unexpected error. Please try again.';
      feedback.style.color = '#b00020';
      console.error(e);
    }
  }

  document.addEventListener('DOMContentLoaded', () => {
    const container = document.getElementById('container');
    const done = document.getElementById('done');
    const denied = document.getElementById('denied');

    if (hasSubmittedFlag()) {
      container.classList.add('hidden');
      denied.classList.remove('hidden');
      // Set full-screen link
      document.getElementById('openFull_denied').href = IFRAME_SRC_FULLSCREEN;
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

    // Fullscreen link on main container
    document.getElementById('openFull').href = IFRAME_SRC_FULLSCREEN;
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

    <div class="msg">
      <strong>Need to correct your vote?</strong><br />
      Please contact an organizer to enter the PIN below to enable re-submission on <em>this device only</em>.
    </div>
    <div class="pin-box">
      <input id="pin_done" class="pin-input" type="password" placeholder="Organizer PIN" autocomplete="off" />
      <button class="btn" onclick="verifyPinAndUnlock('pin_done', 'fb_done')">Submit</button>
    </div>
    <div id="fb_done" class="msg"></div>

    <div class="toolbar" style="justify-content:flex-start;">
      <button class="btn" onclick="location.reload()">Refresh</button>
    </div>
  </div>

  <!-- Denied screen (already marked on this device before opening the form) -->
  <div class="card hidden" id="denied">
    <h2>Already submitted on this device</h2>
    <p class="muted">If you need to re-vote, please contact an organizer to enter the PIN below.</p>

    <div class="toolbar" style="justify-content:flex-start;">
      <a id="openFull_denied" class="btn" target="_blank" rel="noopener">Open full screen</a>
    </div>

    <div class="pin-box">
      <input id="pin_denied" class="pin-input" type="password" placeholder="Organizer PIN" autocomplete="off" />
      <button class="btn" onclick="verifyPinAndUnlock('pin_denied', 'fb_denied')">Submit</button>
    </div>
    <div id="fb_denied" class="msg"></div>

    <div class="toolbar" style="justify-content:flex-start;">
      <button class="btn" onclick="location.reload()">Refresh</button>
    </div>
  </div>
</body>
</html>
