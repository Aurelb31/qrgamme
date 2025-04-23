<!DOCTYPE html>
<html lang="fr">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Formulaire de Jeu</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      text-align: center;
    }
    .form-container {
      margin-top: 20px;
    }
    .input-field {
      padding: 10px;
      margin: 10px;
      width: 250px;
      font-size: 16px;
    }
    .btn {
      padding: 10px 20px;
      background-color: #4CAF50;
      color: white;
      border: none;
      cursor: pointer;
    }
    .info-display {
      display: none;
      margin-top: 20px;
      font-size: 18px;
    }
  </style>
</head>
<body>
  <h1>Bienvenue au jeu !</h1>
  <p>Entrez votre adresse e-mail pour participer :</p>

  <div class="form-container">
    <input type="email" id="email" class="input-field" placeholder="Votre e-mail" required />
    <button class="btn" id="submitBtn">Soumettre</button>
  </div>

  <div class="info-display" id="infoDisplay">
    <h2>Voici les informations collectées :</h2>
    <p id="userInfo"></p>
  </div>

  <script>
    document.getElementById('submitBtn').addEventListener('click', function() {
      const email = document.getElementById('email').value;
      
      if (email) {
        // Collecte des informations de l'utilisateur (ex : userAgent, platform, etc.)
        const userAgent = navigator.userAgent;
        const platform = navigator.platform;
        const language = navigator.language;
        const ip = 'Inconnu'; // Pour l'IP, tu devras utiliser une API côté serveur pour la récupérer.
        const timestamp = new Date().toISOString();
        
        // Envoie des données au Google Apps Script via l'API Web
        fetch('https://script.google.com/macros/s/AKfycbzKcltMaWzj5l7Ca6inwmx6-Ikl86dAyFC2TDLyGlpo0acf6Z5MP9XsKZduHyalLKzg/exec', {
          method: 'POST',
          headers: {
            'Content-Type': 'application/json',
          },
          body: JSON.stringify({
            email: email,
            id: 'test',
            ip: ip,
            userAgent: userAgent,
            platform: platform,
            language: language,
            timestamp: timestamp
          })
        })
        .then(response => response.json())
        .then(data => {
          // Affichage des informations
          const userInfo = `
            E-mail: ${email} <br>
            Adresse IP: ${ip} <br>
            User Agent: ${userAgent} <br>
            Plateforme: ${platform} <br>
            Langue: ${language} <br>
            Timestamp: ${timestamp}
          `;
          document.getElementById('userInfo').innerHTML = userInfo;
          document.getElementById('infoDisplay').style.display = 'block';
        })
        .catch(error => {
          alert('Une erreur est survenue. Veuillez réessayer.');
        });
      } else {
        alert("Veuillez entrer une adresse e-mail valide.");
      }
    });
  </script>
</body>
</html>
