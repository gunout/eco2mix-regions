# ⚡ Dashboard Expert ÉCO2mix — Métropole & DROM-COM

**Analyse interactive du système électrique français — Données officielles RTE et SDES**

[![Version](https://img.shields.io/badge/version-2.0.0-blue.svg)](https://github.com/VOTRE_USER/eco2mix-drom/releases) [![Licence](https://img.shields.io/badge/licence-Licence%20Ouverte%202.0-0055A4.svg)](https://www.etalab.gouv.fr/licence-ouverte-open-licence) [![Made in France](https://img.shields.io/badge/Made%20in-France-000091.svg)](https://www.gouvernement.fr) [![HTML5](https://img.shields.io/badge/HTML5-E34F26?logo=html5&logoColor=white)](https://developer.mozilla.org/fr/docs/Web/HTML) [![JavaScript](https://img.shields.io/badge/JavaScript-F7DF1E?logo=javascript&logoColor=black)](https://developer.mozilla.org/fr/docs/Web/JavaScript) [![Plotly](https://img.shields.io/badge/Plotly.js-2.27.0-3F4F75?logo=plotly&logoColor=white)](https://plotly.com/javascript/) [![GitHub Pages](https://img.shields.io/badge/GitHub%20Pages-live-18753c?logo=github&logoColor=white)](https://VOTRE_USER.github.io/eco2mix-drom/) [![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg)](CONTRIBUTING.md)

[🚀 Démo live](https://VOTRE_USER.github.io/eco2mix-drom/) · [🐛 Signaler un bug](https://github.com/VOTRE_USER/eco2mix-drom/issues) · [✨ Demander une fonctionnalité](https://github.com/VOTRE_USER/eco2mix-drom/issues)

---

## 📋 Table des matières

- [Présentation](#présentation)
- [Fonctionnalités](#fonctionnalités)
- [Sources de données](#sources-de-données)
- [Architecture](#architecture)
- [Installation](#installation)
- [Déploiement](#déploiement)
- [Documentation](#documentation)
- [Structure du projet](#structure-du-projet)
- [Codes indicateurs](#codes-indicateurs)
- [Contribuer](#contribuer)
- [Licence](#licence)

---

## 🎯 Présentation

Ce dashboard interactif permet l'analyse du **système électrique français** à travers deux périmètres complémentaires :

| Périmètre | Source | Granularité | Période | Couverture |
|---|---|---|---|---|
| 🇫🇷 **France métropolitaine** | RTE éCO2mix (ODRE) | 15 minutes | Temps réel | 12 régions + national |
| 🏝️ **DROM-COM** | SDES | Annuelle | 2014 → 2024 | 5 territoires |

Le projet vise à offrir un **outil d'analyse expert** pour chercheurs, journalistes, étudiants et citoyens souhaitant comprendre la **transition énergétique** en France, à la fois sur le territoire métropolitain et dans les territoires d'outre-mer.

---

## ✨ Fonctionnalités

### Mode Métropole (temps réel)

- 📊 **Indicateurs instantanés** : consommation, production totale, mix par filière
- ⏱️ **Analyse temporelle** : 24 h → 14 jours, avec lissage configurable
- 🌱 **Indicateurs environnementaux** : taux de CO₂, part renouvelable, émissions cumulées
- 🇪🇺 **Comparaison européenne** : intensité carbone des pays voisins
- 🔀 **Échanges transfrontaliers** : Angleterre, Espagne, Italie, Suisse, Allemagne-Belgique
- 🔮 **Prévisions** : réalisé vs J-1 vs J, erreur de prévision
- 🔗 **Matrice de corrélation** inter-filières
- 📈 **40+ indicateurs** calculés côté client

### Mode DROM-COM (annuel)

- 🏝️ **5 territoires** : Guadeloupe, Martinique, Guyane, La Réunion, Mayotte
- 📅 **11 années** de données (2014 → 2024)
- ⚡ **Production totale** (E1 = E9 + E2) et mix détaillé
- 🌿 **Part renouvelable** par territoire et évolution
- 📊 **Taux de charge** et facteurs de performance
- 📥 **Export CSV** et PNG

### Commun

- 🌙 **Mode sombre / clair**
- 📥 **Export CSV** (données brutes + tableau récapitulatif)
- 🖼️ **Export PNG** des graphiques
- 📱 **Responsive** (desktop, tablette, mobile)
- ♿ **Accessible** (contrastes élevés, navigation clavier)
- 🎨 **Charte de l'État** (Marianne, tricolore, badges officiels)
- 🔄 **Auto-refresh** toutes les 15 minutes

---

## 📊 Sources de données

### RTE éCO2mix (métropole)

> **Réseau de Transport d'Électricité** — données temps réel du système électrique français

- **Jeu de données** : `eco2mix-national-tr` et `eco2mix-regional-tr`
- **Plateforme** : [ODRE — Open Data Réseaux Énergies](https://odre.opendatasoft.com)
- **API** : `https://odre.opendatasoft.com/api/explore/v2.0/catalog/datasets/`
- **Fréquence de mise à jour** : toutes les 15 minutes
- **Licence** : [Licence Ouverte 2.0](https://www.etalab.gouv.fr/licence-ouverte-open-licence)

### SDES (DROM-COM)

> **Service des données et études statistiques** — Ministère de la Transition écologique

- **Jeu de données** : « Données régionales de production et de consommation finale de l'énergie »
- **Plateforme** : [SDES](https://www.statistiques.developpement-durable.gouv.fr)
- **Format** : fichiers XLSX (traités en JSON dans ce projet)
- **Période** : 2013 → 2024
- **Licence** : [Licence Ouverte 2.0](https://www.etalab.gouv.fr/licence-ouverte-open-licence)

> ⚠️ **Note importante** : RTE ne publie **pas** de données éCO2mix pour les DROM-COM. Ces territoires disposent de réseaux électriques indépendants (« zones non interconnectées ») gérés par EDF SEI, et leurs données sont publiées par le SDES.

---

## 🏗️ Architecture

```text
┌──────────────────────────────────────────────────────────┐
│  GitHub Pages (frontend statique)                        │
│                                                           │
│  index.html                                              │
│  ├─ Mode Métropole ──► API ODRE (RTE)                    │
│  └─ Mode DROM-COM  ──► JSON local (SDES)                 │
│                                                           │
│  drom_energie_2014_2024.json  (données DROM)             │
└──────────────────────────────────────────────────────────┘
                          │
                          ▼ fetch()
┌──────────────────────────────────────────────────────────┐
│  Cloudflare Worker (proxy CORS)                          │
│  https://eco2mix.gunout.workers.dev/                     │
│  └─► https://odre.opendatasoft.com/api/...               │
└──────────────────────────────────────────────────────────┘
```

**Pourquoi un Worker Cloudflare ?** L'API ODRE applique des restrictions CORS strictes. Le Worker agit comme **proxy transparent** pour permettre les appels depuis GitHub Pages.

---

## 🚀 Installation

### Prérequis

- Un compte [GitHub](https://github.com)
- (Optionnel) Un compte [Cloudflare](https://cloudflare.com) pour le proxy CORS

### Étapes

1. **Cloner le dépôt**

   ```bash
   git clone https://github.com/VOTRE_USER/eco2mix-drom.git
   cd eco2mix-drom
   ```

2. **Ouvrir localement**

   ```bash
   # Python 3
   python3 -m http.server 8000
   
   # ou Node.js
   npx serve
   ```

3. **Accéder au dashboard**

   ```text
   http://localhost:8000
   ```

> ⚠️ **Important** : ouvrez le fichier via un **serveur HTTP** (pas `file://`), sinon le `fetch()` du JSON DROM échouera pour des raisons de sécurité CORS.

---

## 🌐 Déploiement

### Déploiement sur GitHub Pages (recommandé)

1. **Pousser le code**

   ```bash
   git add .
   git commit -m "Initial commit"
   git push origin main
   ```

2. **Activer GitHub Pages**
   - Aller dans **Settings** → **Pages**
   - **Source** : `Deploy from a branch`
   - **Branch** : `main` / `/ (root)`
   - Cliquer sur **Save**

3. **Accéder au dashboard**

   ```text
   https://VOTRE_USER.github.io/eco2mix-drom/
   ```

### Déploiement du proxy Cloudflare Worker

1. **Créer un compte Cloudflare** (gratuit)
2. **Aller dans Workers & Pages** → **Create application** → **Create Worker**
3. **Coller le code suivant** :

   ```javascript
   export default {
     async fetch(request) {
       const url = new URL(request.url);
       const apiUrl = url.searchParams.get('apiurl');
       if (!apiUrl) return new Response('Missing apiurl', { status: 400 });
       const response = await fetch(apiUrl);
       const headers = new Headers(response.headers);
       headers.set('Access-Control-Allow-Origin', '*');
       headers.set('Access-Control-Allow-Methods', 'GET, OPTIONS');
       return new Response(response.body, { status: response.status, headers });
     }
   };
   ```

4. **Déployer** et noter l'URL du Worker
5. **Mettre à jour `WORKER_URL`** dans `index.html` si nécessaire

---

## 📖 Documentation

### Structure des données DROM

Exemple d'enregistrement dans `drom_energie_2014_2024.json` :

```json
{
  "region_code": "04",
  "region": "La Réunion",
  "indicateur": "E1",
  "unite": "GWh",
  "annee": 2024,
  "valeur": 3642.9324461718
}
```

**Champs :**

- `region_code` : code du territoire (`01`, `02`, `03`, `04`, `06`)
- `region` : nom du territoire
- `indicateur` : code de l'indicateur (voir section suivante)
- `unite` : unité de mesure (`GWh`, `Unité *`)
- `annee` : année (2014 → 2024)
- `valeur` : valeur numérique

### API JavaScript exposée

```javascript
// Données DROM structurées
ETAT.dromData

// Statistiques
stats([1, 2, 3, 4, 5])           // { mean, std, min, max, sum, count }

// Corrélation
correlation([1,2,3], [2,4,6])     // 1.0

// Moyenne mobile
movingAverage([1,2,3,4,5], 3)     // [null, null, 2, 3, 4]
```

---

## 📁 Structure du projet

```text
eco2mix-drom/
├── index.html                     # Dashboard complet (métropole + DROM)
├── drom_energie_2014_2024.json    # Données SDES DROM-COM
├── README.md                      # Ce fichier
├── LICENSE                        # Licence Ouverte 2.0
└── .gitignore                     # Fichiers à ignorer
```

---

## 🔢 Codes indicateurs

### Nomenclature SDES DROM

| Code | Signification | Formule |
|---|---|---|
| **E1** | Production totale nette d'électricité | **E9 + E2** |
| **E9** | Thermique classique (fioul, charbon, gaz, bagasse, déchets) | — |
| **E2** | Renouvelable électrique primaire | **E4 + E6 + E7** |
| **E4** | Hydraulique | — |
| **E6** | Éolien | — |
| **E7** | Solaire photovoltaïque | — |
| **E3** | EnR thermiques (bagasse, biomasse) | ⚠️ Ne pas utiliser dans le mix |
| **C1** | Consommation primaire | — |
| **C5** | Consommation finale | — |
| **CTR1** | Consommation résidentielle | — |
| **CTR4** | Consommation industrielle | — |
| **CTR5** | Consommation transport | — |

### Nomenclature RTE éCO2mix (métropole)

| Champ | Signification | Unité |
|---|---|---|
| `consommation` | Consommation brute | MW |
| `nucleaire` | Production nucléaire | MW |
| `eolien` | Production éolienne | MW |
| `solaire` | Production solaire | MW |
| `hydraulique` | Production hydraulique | MW |
| `gaz` | Production gaz | MW |
| `charbon` | Production charbon | MW |
| `fioul` | Production fioul | MW |
| `bioenergies` | Production bioénergies | MW |
| `taux_co2` | Intensité carbone | gCO₂/kWh |
| `ech_physiques` | Échanges physiques | MW |

---

## 🤝 Contribuer

Les contributions sont **les bienvenues** !

1. **Fork** le projet
2. **Créer une branche** (`git checkout -b feature/AmazingFeature`)
3. **Commit** les changements (`git commit -m 'Add some AmazingFeature'`)
4. **Push** la branche (`git push origin feature/AmazingFeature`)
5. **Ouvrir une Pull Request**

### Idées de contributions

- 🐛 Correction de bugs
- 🌍 Ajout de nouvelles régions / territoires
- 📊 Nouveaux indicateurs (CO₂ DROM, emploi, etc.)
- 🎨 Améliorations UX/UI
- 🌐 Traductions (anglais, espagnol)
- 📝 Documentation

---

## 📄 Licence

Ce projet est distribué sous **Licence Ouverte 2.0** (Etalab).

Vous êtes libre de :

- ✅ **Partager** — copier, distribuer et communiquer le matériel
- ✅ **Adapter** — remixer, transformer et créer à partir du matériel
- ✅ **Utiliser à des fins commerciales**

À condition de :

- 📌 **Mentionner la paternité** (RTE, SDES, Etalab)
- 📌 **Indiquer les modifications** effectuées
- 📌 **Ne pas utiliser à des fins de désinformation**

Voir le fichier [LICENSE](LICENSE) pour plus de détails.

---

## 🙏 Remerciements

- **[RTE](https://www.rte-france.com)** — Réseau de Transport d'Électricité pour les données éCO2mix
- **[ODRE](https://odre.opendatasoft.com)** — Open Data Réseaux Énergies pour la plateforme API
- **[SDES](https://www.statistiques.developpement-durable.gouv.fr)** — Service des données et études statistiques
- **[Plotly.js](https://plotly.com/javascript/)** — Bibliothèque de visualisation
- **[Etalab](https://www.etalab.gouv.fr)** — Pour la Licence Ouverte
- **[Cloudflare Workers](https://workers.cloudflare.com)** — Pour le proxy CORS

---

**⚡ Fait avec ❤️ pour la transition énergétique française**

---

<div align="center">

### 🇫🇷 Gunout · 2026

![Made in France](https://img.shields.io/badge/Made_in-France-002395?style=flat-square&labelColor=FFFFFF&logo=data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHZpZXdCb3g9IjAgMCA5MDAgNjAwIj48cmVjdCB3aWR0aD0iOTAwIiBoZWlnaHQ9IjYwMCIgZmlsbD0iIzAwMjM5NSIvPjxyZWN0IHdpZHRoPSI5MDAiIGhlaWdodD0iNDAwIiB5PSIxMDAiIGZpbGw9IiNmZmYiLz48cmVjdCB3aWR0aD0iOTAwIiBoZWlnaHQ9IjIwMCIgeT0iNDAwIiBmaWxsPSIjZWQyOTM5Ii8+PC9zdmc+)
![GitHub](https://img.shields.io/badge/GitHub-gunout-181717?style=flat-square&logo=github&logoColor=white)
![Year](https://img.shields.io/badge/2026-ED2939?style=flat-square&labelColor=FFFFFF)

<sub>© 2026 <strong>Gunout</strong> — Tous droits réservés.</sub>

</div>
