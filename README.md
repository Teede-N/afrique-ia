# AfrIA — Comprends l'IA. Construis l'avenir.

![JavaScript](https://img.shields.io/badge/JavaScript-F7DF1E?logo=javascript&logoColor=black)
![PWA](https://img.shields.io/badge/PWA-installable-5A0FC8?logo=pwa&logoColor=white)
![Serverless](https://img.shields.io/badge/API-serverless-000000?logo=vercel)
![Langues](https://img.shields.io/badge/langues-FR_·_EN_·_AR_·_YO-0e5c3f)

> Plateforme **gratuite** pour former la jeunesse à l'intelligence artificielle : **53 modules** en accès libre, une **filière universitaire**, des **outils pour les enseignants** et les établissements. Seul le certificat de réussite est payant.

<p align="center">
  <a href="https://afria-plateforme.vercel.app"><img src="https://img.shields.io/badge/▶_Essayer_en_ligne-afria--plateforme-2a78d6?style=for-the-badge" alt="Essayer en ligne"></a>
  <a href="media/demo-afria.mp4"><img src="https://img.shields.io/badge/🎬_Vidéo_HD-MP4-eb6834?style=for-the-badge" alt="Vidéo HD"></a>
</p>

<p align="center">
  <img src="media/demo-afria.gif" alt="Démo d'AfrIA : accueil, formations, modules, ressources, masterclass, actualités IA en direct" width="760">
</p>

<p align="center"><sub>Démo animée. Cliquez sur <b>Vidéo HD</b> pour la version MP4, ou sur <b>Essayer en ligne</b> pour tester le site.</sub></p>

### 📸 Captures d'écran

<table>
  <tr>
    <td><img src="media/capture-1.png" alt="Accueil"></td>
    <td><img src="media/capture-2.png" alt="Choisis ta formation"></td>
    <td><img src="media/capture-3.png" alt="Actualités IA en direct"></td>
  </tr>
</table>

---

## Sommaire

1. [Mission](#1-mission)
2. [L'offre en chiffres](#2-loffre-en-chiffres)
3. [Fonctionnalités](#3-fonctionnalités)
4. [Parcours utilisateur](#4-parcours-utilisateur)
5. [Architecture](#5-architecture)
6. [Zoom technique : l'API d'actualités](#6-zoom-technique--lapi-dactualités)
7. [Pensé pour le terrain](#7-pensé-pour-le-terrain)
8. [Feuille de route](#8-feuille-de-route)

---

## 1. Mission

L'IA transforme tous les métiers. Pourtant, les ressources de qualité **en français** et **adaptées au contexte africain** restent rares, et souvent payantes. AfrIA offre un point d'entrée **gratuit, accessible 24 h/24 et multilingue**, pour les apprenants comme pour ceux qui les forment.

---

## 2. L'offre en chiffres

| Indicateur | Valeur |
|---|---|
| Modules en accès libre | **53** |
| Coût de l'apprentissage | **0 €** |
| Langues de l'interface | **4** (français, anglais, arabe, yoruba) |
| Parcours thématiques | **10** (fondamentaux, IA générative, masterclass no-code, IA sectorielle, éthique, IA avancée, filière universitaire, enseignants et écoles, ressources, certificat) |
| Masterclass avancée | **40 modules** en 4 piliers, environ 160 h estimées |

---

## 3. Fonctionnalités

| Pour qui | Ce qu'AfrIA propose |
|---|---|
| 🎓 **Apprenants** | 53 modules filtrables par thème (assistants, création, agriculture, santé, éducation, emploi, éthique, avancé, université), compte gratuit, certificat de réussite en option |
| 🧑‍🏫 **Enseignants** | Espace dédié pour déposer des supports de cours et suivre la progression des élèves |
| 🏫 **Écoles et entreprises** | Licence établissement multi-comptes, filière universitaire incluse, demande de démonstration |
| 📚 **Tous** | Ressources téléchargeables gratuitement : programme universitaire (PDF), support PowerPoint, 41 modules en PDF (français et anglais) |
| 📰 **Curieux** | Fil d'**actualités IA en direct**, mis à jour automatiquement |
| 🚀 **Avancés** | Masterclass IA & No-Code de 40 modules, avec vidéos d'approfondissement et un projet de fin de formation |

---

## 4. Parcours utilisateur

```mermaid
flowchart LR
    A[Accueil] -->|Choisis ta formation| B[10 parcours]
    B --> C[53 modules<br/>filtrés par thème]
    C -->|compte gratuit| D[Espace apprenant]
    D --> E[Certificat<br/>de réussite]
    A --> F[Ressources PDF<br/>et PowerPoint]
    A --> G[Actualités IA<br/>en direct]
```

L'expérience reste **simple** : une page d'accueil claire, des cartes de couleur pour choisir sa formation en un clic, des filtres par thème et un seul formulaire pour créer son compte.

---

## 5. Architecture

```mermaid
flowchart LR
    U((Apprenant<br/>mobile ou PC)) --> PWA[Application PWA<br/>site statique]
    PWA -->|fetch| API["/api/news<br/>fonction serverless"]
    API -->|RSS| G[Flux d'actualités IA<br/>en français]
    API -->|JSON mis en cache 15 min| PWA
```

| Élément | Rôle |
|---|---|
| `manifest.json` | Rend le site installable (PWA) : nom, icônes, couleurs, affichage autonome |
| `api/news.js` | Fonction serverless : flux RSS vers JSON propre, avec cache CDN |
| `build.js` | Assemblage du site au moment du déploiement |

---

## 6. Zoom technique : l'API d'actualités

**Problème** : un navigateur ne peut pas lire directement un flux RSS d'un autre domaine (CORS), et interroger la source à chaque visite serait lent et coûteux.

**Solution** : une **fonction serverless** sert d'intermédiaire. Elle extrait les 8 articles les plus récents (titre, lien, date, source), nettoie les balises CDATA et répond en JSON.

**Stratégie de cache** : `Cache-Control: s-maxage=900, stale-while-revalidate=1800`

| Directive | Effet |
|---|---|
| `s-maxage=900` | Le CDN garde la réponse **15 minutes** : la source n'est interrogée qu'environ 4 fois par heure, quel que soit le nombre de visiteurs |
| `stale-while-revalidate=1800` | Pendant 30 minutes supplémentaires, le visiteur reçoit **immédiatement** la version en cache pendant qu'elle se rafraîchit en arrière-plan |

Avec $N$ visiteurs par heure, le nombre d'appels à la source passe de $N$ à environ 4 : la charge devient **indépendante du trafic**.

---

## 7. Pensé pour le terrain

| Contrainte du public | Réponse |
|---|---|
| Connexions mobiles lentes ou coûteuses | Site statique léger, actualités mises en cache côté CDN, PDF téléchargeables |
| Usage majoritairement sur smartphone | PWA installable, affichage autonome |
| Diversité linguistique | Interface en français, anglais, arabe et yoruba |
| Pas de budget pour se former | Tous les modules sont gratuits ; seul le certificat est payant |

---

## 8. Feuille de route

- [x] 53 modules en ligne, 10 parcours, filière universitaire
- [x] Espace enseignant et offre écoles / entreprises
- [x] Ressources téléchargeables (PDF, PowerPoint)
- [x] Actualités IA en direct et mise à jour mensuelle automatisée
- [x] Cours AfrIA intégrés à l'espace Apprentissage de [Teedaïk](https://github.com/Teede-N/teedaik-showcase)
- [ ] Mode hors ligne complet (service worker)
- [ ] Quiz auto-corrigés et tableau de progression détaillé

---

## 👤 Auteur

**Teede Augustin NOUDAMADJO**, fondateur d'AfrIA · Data Scientist Junior
Licence de mathématiques (École normale supérieure, Bénin) · Master Big Data & IA (ESTIAM Lyon)
[LinkedIn](https://www.linkedin.com/in/teede-noudamadjo) · teedeaugustinnoudamadjo41@gmail.com
