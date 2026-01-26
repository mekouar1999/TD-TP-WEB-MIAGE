# Séance de TD — JavaScript Front-End & Back-End avec Node.js & Express

## Objectif global des TD

Avant de commencer JavaScript et Node.js, une **remise à niveau en HTML et CSS** a volontairement été effectuée lors des TD et TP précédents.

Cette décision pédagogique est essentielle :

* Il est **impossible de maîtriser JavaScript Front-End** sans comprendre le HTML, le CSS et la structure d’une page
* La manipulation du **DOM** constitue la base même de JavaScript côté client

Ces bases étant désormais acquises, ce TD marque une **nouvelle étape** du module.

À partir d’aujourd’hui (27 janvier) et jusqu’aux derniers TD du semestre, l’objectif est de :

* Comprendre clairement la **différence entre JavaScript et Node.js**
* Maîtriser JavaScript côté **client**
* Apprendre JavaScript côté **serveur** avec Node.js
* Construire progressivement une vraie logique **Full Stack**

Ce TD s’inscrit dans une continuité : ce qui n’est pas terminé en TD sera repris et approfondi en TP, notamment lors du développement de l’application avec vos jeux que vous devriez réaliser et lier à votre app web pour votre projet final.

---

## Organisation du TD

Ce TD marque le **début officiel du travail en JavaScript**, aussi bien côté client que côté serveur. 

* **Démarrage** : 27 janvier ( pour le premier groupe le 27 (TD2) , et le 29 janvier pour le deuxieme groupe (TD1) 
* **Objectif des TD** : comprendre au maximum, les bases et les concepts clés de JavaScript ( +++ ) côté Front-End et côté Back-End ( ++ )

Les TD servent avant tout à :

* introduire les notions
* expliquer les concepts
* comprendre la logique globale

Lors des **TP**, ces notions seront naturellement **remises en pratique** à travers le développement d’applications et de projets concrets.

En développant une application réelle, vous utiliserez forcément :

* JavaScript côté client
* JavaScript côté serveur (Node.js)
* une base de données ( Je vais vous montrer comment utiliser LocalStorage étant un stockage côté client et donc côté navigateur, et puis on fera rapidement du Mongodb/mongoose pour pouvoir stocker vos données dans une vraie base de donnée.

Il est donc normal de ne pas tout maîtriser immédiatement en TD : la compréhension se consolide progressivement par la pratique en TP.

---

# PARTIE 1 — JavaScript côté Client (Front-End)

## 1️ Rappel : JavaScript dans le navigateur

JavaScript côté Front-End s’exécute **dans le navigateur**.

Il permet de :

* Manipuler le HTML (DOM)
* Gérer les événements (click, submit, etc.)
* Dynamiser l’interface utilisateur

### Environnement Front-End

* Navigateur
* Accès au DOM
* Accès à `window`, `document`

---

## 2 Exemple simple JavaScript Front-End

### HTML

```html
<button id="btn">Clique-moi</button>
<p id="result"></p>
```

### JavaScript

```js
document.getElementById("btn").addEventListener("click", () => {
  document.getElementById("result").textContent = "JavaScript côté client 🚀";
});
```

---

##  Exercices Front-End — DOM & Events

Les exercices suivants sont conçus pour explorer **l’ensemble des événements JavaScript importants côté client** et comprendre comment interagir avec le DOM de manière dynamique.

---

### Exercice 1 — Interaction simple (click)

Créer une `div` visible à l’écran.

Comportement attendu :

* Au clic sur la div, sa couleur de fond change
* À chaque clic, une couleur différente est appliquée

Notions travaillées :

* `addEventListener('click')`
* manipulation du style via JavaScript

---

### Exercice 2 — Suivi de la souris (mousemove)

Créer une zone d’affichage qui indique en temps réel :

* la position X de la souris
* la position Y de la souris

Bonus : déplacer un petit élément visuel en fonction de la position de la souris.

Notions travaillées :

* `mousemove`
* objet `event`
* interaction temps réel

---

### Exercice 3 — Réaction au scroll (scroll)

Créer une page suffisamment longue pour permettre le scroll.

Comportement attendu :

* Lorsque l’utilisateur scroll :

  * changer la couleur du header
  * afficher un message indiquant le niveau de scroll

Notions travaillées :

* `scroll`
* `window.scrollY`

---

### Exercice 4 — Formulaire et submit

Créer un formulaire avec :

* un champ texte
* un bouton de validation

Comportement attendu :

* empêcher le rechargement de la page
* afficher la valeur saisie sous le formulaire
* afficher un message d’erreur si le champ est vide

Notions travaillées :

* `submit`
* `preventDefault()`
* validation simple

---

### Exercice 5 — Mini calculatrice interactive

Créer une mini calculatrice avec :

* deux champs numériques
* des boutons `+`, `-`, `×`, `/`
* une zone d’affichage du résultat

Contraintes :

* vérifier que les champs ne sont pas vides
* gérer les erreurs (division par zéro)

Notions travaillées :

* logique JavaScript
* récupération des valeurs
* événements multiples

📌 Objectif global : maîtriser les événements JavaScript (`click`, `mousemove`, `scroll`, `submit`) et la manipulation avancée du DOM.

---

# 🟨 PARTIE 2 — JavaScript côté Serveur (Back-End)

## 3 Introduction à Node.js

Node.js permet d’exécuter JavaScript **en dehors du navigateur**, sur un **serveur**.

### Différences fondamentales

| JavaScript Front-End | JavaScript Back-End |
| -------------------- | ------------------- |
| Navigateur           | Serveur             |
| DOM                  | ❌                   |
| UI / UX              | Logique métier      |
| HTML / CSS           | API / Données       |

---

## 4 Premier script Node.js

### server.js

```js
console.log("JavaScript côté serveur 🚀");
```

```bash
node server.js
```

---

## 5 Modules en Node.js

### math.js

```js
function addition(a, b) {
  return a + b;
}

function soustraction(a, b) {
  return a - b;
}

module.exports = {
  addition,
  soustraction
};
```

### server.js

```js
const math = require("./math");

console.log(math.addition(4, 6));
```

📌 Notions importantes :

* `require`
* `module.exports`
* séparation du code

---

# 🟦 PARTIE 3 — Express.js & Serveur Web

## 6 Pourquoi Express.js ?

* Simplifie Node.js
* Gestion claire des routes
* Standard professionnel

---

## 7 Initialisation d’un projet Express

```bash
npm init -y
npm install express
```

### server.js

```js
const express = require("express");
const app = express();

app.get("/", (req, res) => {
  res.send("Hello Express 👋");
});

app.listen(3000, () => {
  console.log("Server running on http://localhost:3000");
});
```

---

## 8 Les routes avec Express

### Route GET

```js
app.get("/api/users", (req, res) => {
  res.json([
    { id: 1, name: "Alice" },
    { id: 2, name: "Bob" }
  ]);
});
```

### Paramètres de route

```js
app.get("/api/users/:id", (req, res) => {
  res.send(req.params.id);
});
```

---

## 9 Middleware & POST

```js
app.use(express.json());

app.post("/api/users", (req, res) => {
  res.json({
    message: "Utilisateur reçu",
    data: req.body
  });
});
```

---

# 🟪 PARTIE 4 — Librairies essentielles Node.js

* **express** → serveur web
* **nodemon** → redémarrage automatique
* **cors** → communication front/back
* **dotenv** → variables d’environnement
* **mongoose** → MongoDB (plus tard)

Installation nodemon :

```bash
npm install -D nodemon
```

---

# 🟥 PARTIE 5 — Architecture & Modèle MVC

## 10 Pourquoi structurer son backend ?

Objectifs :

* Lisibilité
* Scalabilité
* Maintenance

---

## 11 Modèle MVC (simplifié)

* **Model** : gestion des données
* **Controller** : logique métier
* **Routes** : points d’entrée API

### Exemple de structure

```
backend/
├── controllers/
├── routes/
├── models/
├── server.js
```

---

# 🟩 PARTIE 6 — Lien Front-End ↔ Back-End

## 12 fetch() côté Front-End

```js
fetch("http://localhost:3000/api/users")
  .then(res => res.json())
  .then(data => console.log(data));
```

📌 Principe fondamental :

> Le Front consomme une API, le Back fournit des données.

---

# 🏁 Conclusion et vision du module

Ce TD n’est pas un exercice isolé.
Il constitue la **base technique pour le rendu finale de votre app web avec vos jeux vidéos **  .

Les compétences vues ici seront directement réutilisées dans :

* le développement de votre **application web avec vos jeux vidéos 2 ou 3 à voir avec M.BUFFA **

 # Othman MEKOUAR - Chargé de TD/TP du module Application WEB - MIAGE 

