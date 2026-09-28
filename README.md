<p align="center">
  <img src="logo_thelma.png" alt="Thelma" width="260">
</p>

<h3 align="center">La loi, remise dans son contexte, pour les élus qui la découvrent</h3>

<p align="center">
  <a href="https://thelma-tertrais.github.io/HackathonAssembl-eNat/"><strong>👉 Essayer la démo en ligne</strong></a>
</p>

<p align="center">
  Projet réalisé lors du <a href="https://hackathon2026.assemblee-nationale.fr/defis/1194e3c0-50f4-4ff0-be00-9e1d065aaba8">Hackathon 2026 de l'Assemblée nationale</a>
  « Le parcours de la loi : vers une IA de confiance » (3 et 4 juillet 2026)
</p>

---

![Tableau de bord Thelma](hackathon-an-2026/images/02-dashboard.png)

## Le projet

**Thelma** est une plateforme pour les élus locaux qui traduit les lois en enjeux locaux et en bénéfices concrets. Elle leur permet de :

- **monter rapidement en compétence**, de manière ludique ;
- **s'approprier la loi et la communiquer** à leurs administrés ;
- **décliner l'intention du législateur en actions concrètes** pendant leur mandat.

Le cas d'étude choisi est le **ZAN (zéro artificialisation nette)**. Il s'agit d'une intention unique et datée : réduire l'artificialisation des sols, objectif posé par la loi Climat et Résilience du 22 août 2021. Cette intention a ensuite été précisée, corrigée et contestée par plusieurs lois, décrets et propositions, jusqu'à aujourd'hui.

## Ce que fait la démo

| | |
|---|---|
| **Généalogie des textes** | Lois, décrets et propositions en cours liés au ZAN, avec leur catégorie (fondatrice, préparatoire, sectorielle…) et leurs relations (modifie, précise, remplace…). |
| **Feuille de route du mandat** | Frise chronologique et vue Gantt des échéances, y compris ce qui reste incertain tant que la proposition de loi TRACE n'est pas définitivement votée. |
| **Fiches argumentaires** | Pourquoi le texte existe, objections fréquentes et réponses, marges de manœuvre concrètes pour la commune. |

<details>
<summary><strong>Voir les captures d'écran</strong></summary>

| Inscription | Feuille de route ZAN (Gantt) |
|---|---|
| ![Inscription](hackathon-an-2026/images/01-inscription.png) | ![Gantt](hackathon-an-2026/images/03-gantt.png) |
| **Liste des textes ZAN** | **Détail d'un texte** |
| ![Textes](hackathon-an-2026/images/04-lois.png) | ![Détail](hackathon-an-2026/images/05-modale-loi.png) |
| **Argumentaire face aux administrés** | **Frise chronologique du mandat** |
| ![Argumentaire](hackathon-an-2026/images/06-argumentaire.png) | ![Frise](hackathon-an-2026/images/07-frise-mandat.png) |

</details>

## Comment ça marche

Le projet combine une base de démonstration et un pipeline automatisé :

1. **Extraction** : un script découpe le texte intégral d'une loi ou d'un décret (PDF).
2. **Synthèse par IA** : un modèle de langage (Groq, `llama-3.3-70b-versatile`) en extrait une fiche structurée (titre, date, catégorie, résumé, relations), avec la consigne stricte de **ne jamais inventer une relation** qui n'est pas explicitement mentionnée dans le texte.
3. **Injection** : la base générée alimente l'application web.

Un second pipeline ingère des sources juridiques connexes (codes, jurisprudence, droit européen) et les classe par **force juridique** (contraignant ou interprétatif). Sur les cas ambigus, le modèle indique un niveau de confiance explicite plutôt que de trancher à l'aveugle.

Par souci de fiabilité, **les échéances de mandat et les argumentaires ne sont jamais publiés sans relecture humaine.**

Le détail technique est dans la [méthodologie](hackathon-an-2026/docs/methodologie.md).

## Lancer le projet

**L'application** est un site statique : ouvrez simplement `index.html` dans un navigateur, ou utilisez la [démo en ligne](https://thelma-tertrais.github.io/HackathonAssembl-eNat/).

**Le pipeline de données** nécessite Python et une clé API Groq :

```bash
pip install -r requirements.txt
export GROQ_API_KEY=votre_cle

# Dossier contenant les PDF et un manifest.json (voir manifest.exemple.json)
python3 generer_tout.py mon_dossier_pdfs/
python3 injecter_dans_app.py
```

## Données utilisées

- Dossiers législatifs de l'Assemblée nationale (législature courante)
- Amendements déposés à l'Assemblée nationale (17e législature)
- Codes, lois et règlements consolidés, et édition « Lois et décrets » du Journal officiel
- API et serveur MCP d'accès unifié Parlement / Législation / Service public

## Équipe

Défi porté par **Nicolas Thouvenin**, avec Philippe Cases, **Thelma Tertrais**, Amine Abouhodaifa, Jérome Funamal, Carlos Holguin, Guillaume de la Lubie, Adewoye Shakir Oyeossi et Alex Sant André.

Ce dépôt est un fork de [FCL-PE/HackathonAN](https://github.com/FCL-PE/HackathonAN), qui ajoute une version déployée de l'application.

