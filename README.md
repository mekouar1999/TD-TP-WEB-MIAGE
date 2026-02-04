
# Node ( Séance du Mercredi 5 ( Dernier TD )

Node.js et JavaScript ne sont PAS la même chose.

JavaScript est un langage de programmation.
À l’origine, il s’exécute uniquement dans le navigateur (Chrome, Firefox, etc.).
Il sert à manipuler le HTML, le CSS, gérer les clics, formulaires et animations.

Node.js est un environnement d’exécution de JavaScript côté serveur.
Il permet d’exécuter du JavaScript en dehors du navigateur, sur un serveur.

Avec Node.js, JavaScript peut :
- Créer un serveur web
- Accéder aux fichiers du système
- Se connecter à une base de données
- Créer des API
- Gérer des utilisateurs, des scores, des paiements

Pourquoi utiliser Node.js :
- Un seul langage pour le frontend et le backend
- Très rapide (asynchrone, non bloquant)
- Très utilisé dans le web moderne
- Parfait pour les API, jeux, temps réel
- Immense écosystème via npm

Node.js fonctionne avec :
- Express : création simple de serveurs et API
- MongoDB : base de données NoSQL très utilisée avec Node.js
- JSON : format d’échange des données

Dans ce projet :
- Le frontend envoie un score
- Le backend Node.js reçoit ce score via une API
- MongoDB stocke le score
- Le backend renvoie les scores au frontend

Objectif pédagogique :
Maîtriser les bases de Node.js :
- créer un serveur
- créer une API REST
- comprendre les requêtes HTTP
- connecter une base de données
- relier frontend et backend

```text
game-scores-app/
├── backend/
│   ├── server.js
│   ├── package.json
│   ├── .env
│   ├── config/
│   │   └── db.js
│   ├── models/
│   │   └── Score.js
│   └── routes/
│       └── score.routes.js
│
└── frontend/
    ├── index.html
    ├── style.css
    └── game.js
```

```bash
cd backend
npm init -y
npm install express mongoose dotenv
npm install nodemon --save-dev
```

```env
PORT=5000
MONGO_URI=mongodb+srv://username:password@cluster.mongodb.net/game-scores
```

```js
const express = require("express");
const dotenv = require("dotenv");
const connectDB = require("./config/db");

dotenv.config();
connectDB();

const app = express();
app.use(express.json());

app.use("/api/scores", require("./routes/score.routes"));

app.listen(process.env.PORT || 5000);
```

```js
const mongoose = require("mongoose");

module.exports = async () => {
  await mongoose.connect(process.env.MONGO_URI);
};
```

```js
const mongoose = require("mongoose");

module.exports = mongoose.model(
  "Score",
  new mongoose.Schema(
    { playerName: String, score: Number },
    { timestamps: true }
  )
);
```

```js
const router = require("express").Router();
const Score = require("../models/Score");

router.post("/", async (req, res) => {
  const score = await Score.create(req.body);
  res.status(201).json(score);
});

router.get("/", async (req, res) => {
  const scores = await Score.find().sort({ score: -1 }).limit(10);
  res.json(scores);
});

module.exports = router;
```

```html
<!DOCTYPE html>
<html lang="fr">
<head>
  <meta charset="UTF-8">
  <title>Game</title>
  <link rel="stylesheet" href="style.css">
</head>
<body>

<form id="scoreForm">
  <input id="playerName" placeholder="Nom" required>
  <input id="score" type="number" placeholder="Score" required>
  <button>Envoyer</button>
</form>

<ul id="scores"></ul>

<script src="game.js"></script>
</body>
</html>
```

```css
body {
  font-family: Arial;
  padding: 40px;
}
```

```js
const form = document.getElementById("scoreForm");
const list = document.getElementById("scores");

fetch("http://localhost:5000/api/scores")
  .then(r => r.json())
  .then(scores => {
    list.innerHTML = "";
    scores.forEach(s => {
      const li = document.createElement("li");
      li.textContent = `${s.playerName} : ${s.score}`;
      list.appendChild(li);
    });
  });

form.addEventListener("submit", async e => {
  e.preventDefault();

  await fetch("http://localhost:5000/api/scores", {
    method: "POST",
    headers: { "Content-Type": "application/json" },
    body: JSON.stringify({
      playerName: playerName.value,
      score: score.value
    })
  });

  location.reload();
});
```

```bash
cd backend
npx nodemon server.js
```

### Maitenant que votre back est lié avec votre back, retournez finir vos jeux !
