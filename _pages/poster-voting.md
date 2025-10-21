---
title: Poster Voting
layout: sub
permalink: /poster-voting/
---

<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width,initial-scale=1" />
<title>2025 IEEE CyberSciTech/DASC/PICom/CBDCom – Voting for the Best Poster</title>
<style>
  html, body {
    font-family: system-ui, -apple-system, "Segoe UI", Roboto, "Helvetica Neue", Arial;
    margin: 0;
    padding: 0;
    background: #fafafa;
    color: #111;
  }
  .card {
    max-width: 800px;
    margin: 0 auto;
    padding: 20px 16px;
    background: #fff;
    border-radius: 12px;
    box-shadow: 0 1px 4px rgba(0,0,0,0.1);
  }
  h2 {
    font-size: 1.25rem;
    line-height: 1.4;
    margin-bottom: 8px;
  }
  .muted {
    color: #555;
    font-size: 0.95rem;
    line-height: 1.5;
  }
  .hidden { display: none; }
  .btn {
    display: inline-block;
    margin-top: 8px;
    padding: 10px 18px;
    border: 1px solid #ccc;
    border-radius: 8px;
    background: #f8f8f8;
    cursor: pointer;
    font-size: 0.95rem;
  }
  .btn:hover { background: #eee; }
  iframe {
    width: 100%;
    height: 90vh; /* occupy most of the screen height */
    border: 0;
    margin-top: 12px;
  }
  @media (max-width: 600px) {
    .card { border-radius: 0; box-shadow: none; }
    body { background: #fff; }
    h2 { font-size: 1.1rem; }
    .muted { font-size: 0.9rem; }
  }
</style>
<script>
  const FORM_ID = '1FAIpQLSdhRpCrafvTMqVIfcXWNh_ZoL0XwLIxcUqnzFwx-ySSpH6vhQ';
  const IFRAME_SRC = `https://docs.google.com/forms/d/e/${FORM_ID}/viewform?embedded=true`;

  function setSubmittedFlag(days = 365) {
    try { localStorage.setItem(`form_${FORM_ID}_submitted`, '1'); } catch {}
    const d = new Date();
    d.setTime(d.getTime() + days*24*60*60*1000);
    document.cookie = `form_${FORM_ID}_submitted=1; expires=${d.toUTCString()}; path=/; SameSite=Lax`;
  }
  function hasSubmittedFlag() {
    try { if (localStorage.getItem(`form_${FORM_ID}_submitted`) === '1') return true; } catch {}
    return document.cookie.split('; ').some(row => row.startsWith(`form_${FORM_ID}_submitted=`));
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
    iframe.src = IFRAME_SRC;

    iframe.addEventListener('load', () => {
      loadCount++;
      if (loadCount >= 2) {
        setSubmittedFlag();
        container.classList.add('hidden');
        done.classList.remove('hidden');
      }
    });

    document.getElementById('frameHost').appendChild(iframe);

    document.getElementById('markSubmitted').addEventListener('click', () => {
      setSubmittedFlag();
      container.classList.add('hidden');
      done.classList.remove('hidden');
    });
  });
</script>
</head>
<body>
  <div class="card" id="container">
    <h2>2025 IEEE CyberSciTech/DASC/PICom/CBDCom – Voting for the Best Poster</h2>
    <p class="muted">
      To prevent duplicate voting on this device, a local marker (Cookie/LocalStorage) will be set after a successful submission.
    </p>
    <div id="frameHost"></div>
    <p class="muted">
      If your submission completes but the page does not auto-detect it, click:
      <button id="markSubmitted" class="btn">I have submitted</button>
    </p>
  </div>

  <div class="card hidden" id="done">
    <h2>Thank you for your submission!</h2>
    <p class="muted">Your vote has been recorded. If you need to modify it, please contact the organizing committee.</p>
    <button class="btn" onclick="location.reload()">Refresh</button>
  </div>

  <div class="card hidden" id="denied">
    <h2>Already submitted on this device</h2>
    <p class="muted">If you need to re-vote due to an error, please contact the organizing committee.</p>
    <button class="btn" onclick="location.reload()">Refresh</button>
  </div>
</body>
</html>
