<!DOCTYPE html>
<html lang="fr">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Jeu - Infos Collectées</title>
  <style>
    body {
      font-family: "Segoe UI", Tahoma, Geneva, Verdana, sans-serif;
      text-align: center;
      background-color: #f0f4f8;
      color: #333;
      padding: 20px;
    }
    .form-container {
      margin-top: 30px;
    }
    .input-field {
      padding: 10px;
      margin: 10px;
      width: 250px;
      font-size: 16px;
      border-radius: 5px;
      border: 1px solid #ccc;
    }
    .btn {
      padding: 10px 25px;
      background-color: #0078d4;
      color: white;
      border: none;
      border-radius: 5px;
      cursor: pointer;
    }
    .btn:hover {
      background-color: #005ea3;
    }
    .info-display {
      display: none;
      margin-top: 30px;
      font-size: 18px;
      background-color: white;
      padding: 20px;
      border-radius: 10px;
      box-shadow: 0 4px 10px rgba(0,0,0,0.1);
    }
  </style>
</head>
<body>
  <h1>Bienvenue dans notre jeu !</h1>
  <p>Indique ton prénom et ton nom pour participer :</p>

  <div class="form-container">
    <input type="text" id="firstname" class="input-field" placeholder="Prénom" required />
    <input type="text" id="lastname" class="input-field" placeholder="Nom" required />
    <button class="btn" id="submitBtn">Soumettre</button>
  </div>

  <div class="info-display" id="infoDisplay">
    <h2>Ah, on t’a bien eu 😏</h2>
    <p>Voici ce que l’on a pu récupérer sur toi :</p>
    <p id="userInfo"></p>
  </div>

  <script>
    document.getElementById('submitBtn').addEventListener('click', () => {
      const firstname = document.getElementById('firstname').value.trim();
      const lastname = document.getElementById('lastname').value.trim();

      if (firstname && lastname) {
        const userAgent = navigator.userAgent;
        const platform = navigator.platform;
        const language = navigator.language;
        const ip = "Inconnue"; // Peut être enrichie côté serveur
        const timestamp = new Date().toISOString();

        fetch("https://script.google.com/macros/s/AKfycbw7jSeIn449nXv9SoaW62UOM1n_kMh1S4ncT0d5SIInxr50SnDR-YQF2BbXwQRyPIym/exec", {
          method: "POST",
          headers: {
            "Content-Type": "application/json"
          },
          body: JSON.stringify({
            firstname,
            lastname,
            id: "demo", // ou un identifiant unique
            ip,
            userAgent,
            platform,
            language,
            timestamp
          })
        })
        .then(res => res.json())
        .then(data => {
          const display = `
            Nom : ${lastname}<br>
            Prénom : ${firstname}<br>
            Adresse IP : ${ip}<br>
            User Agent : ${userAgent}<br>
            Plateforme : ${platform}<br>
            Langue : ${language}<br>
            Timestamp : ${timestamp}
          `;
          document.getElementById("userInfo").innerHTML = display;
          document.getElementById("infoDisplay").style.display = "block";
        })
        .catch(err => {
          alert("Une erreur est survenue. Veuillez réessayer.");
          console.error(err);
        });
      } else {
        alert("Merci de remplir tous les champs.");
      }
    });
  </script>
</body>
</html>
