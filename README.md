<!DOCTYPE html>
<html lang="fr">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Jeu QR Code</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      background-color: #111;
      color: #fff;
      text-align: center;
      padding: 2em;
    }
    h1 {
      font-size: 2em;
      animation: fadeIn 1.5s ease-in-out;
    }
    ul {
      list-style: none;
      padding: 0;
      font-size: 1.2em;
    }
    li {
      margin: 1em 0;
      animation: slideIn 0.5s ease forwards;
    }
    .cam-flash {
      width: 100%;
      height: 100vh;
      background: #fff;
      position: fixed;
      top: 0;
      left: 0;
      opacity: 0;
      z-index: 9999;
      pointer-events: none;
      animation: camFlash 0.8s ease-in-out 2s forwards;
    }
    @keyframes camFlash {
      0% { opacity: 0; }
      50% { opacity: 1; }
      100% { opacity: 0; }
    }
    @keyframes fadeIn {
      from { opacity: 0; }
      to { opacity: 1; }
    }
    @keyframes slideIn {
      from { opacity: 0; transform: translateY(20px); }
      to { opacity: 1; transform: translateY(0); }
    }
  </style>
</head>
<body>
  <div class="cam-flash"></div>
  <h1>Merci d'avoir scanné le QR code !</h1>
  <p>Vous pensiez que ce QR code était anodin ? Voici ce que nous avons pu apprendre sur vous...</p>
  <ul id="infos"></ul>

  <script>
    async function getClientInfo() {
      const infoList = document.getElementById('infos');
      const urlParams = new URLSearchParams(window.location.search);
      const userID = urlParams.get('user') || 'Inconnu';
      const userAgent = navigator.userAgent;
      const language = navigator.language;
      const platform = navigator.platform;
      let ip = '...';

      infoList.innerHTML = `
        <li><strong>ID utilisateur :</strong> ${userID}</li>
        <li><strong>Appareil :</strong> ${platform}</li>
        <li><strong>Navigateur :</strong> ${userAgent}</li>
        <li><strong>Langue :</strong> ${language}</li>
        <li><strong>Adresse IP :</strong> en cours...</li>
      `;

      try {
        const response = await fetch('https://api.ipify.org?format=json');
        const data = await response.json();
        ip = data.ip;
        infoList.querySelectorAll('li')[4].innerHTML = `<strong>Adresse IP :</strong> ${ip}`;
      } catch (err) {
        console.error('Erreur IP:', err);
      }

      // ENREGISTREMENT VERS GOOGLE SHEET via webhook Apps Script
      fetch('https://script.google.com/macros/s/AKfycbyeIWjJc7y7eZm6AbLORbswcmGcWmzDMB3nks2n1197Ip2K-UzLdy6AKzw751z0cD24/exec', {
        method: 'POST',
        mode: 'no-cors',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({
          id: userID,
          ip: ip,
          userAgent: userAgent,
          platform: platform,
          language: language,
          timestamp: new Date().toISOString()
        })
      });
    }

    getClientInfo();
  </script>
</body>
</html>
