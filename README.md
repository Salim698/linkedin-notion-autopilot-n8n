# LinkedIn Notion Autopilot - n8n + Gemini

Workflow n8n qui transforme une base Notion en machine à posts LinkedIn : tu écris une idée, Gemini rédige le post, tu valides, et le workflow publie.

![Workflow n8n](images/workflow.png)

## Fonctionnement

Le workflow surveille une base de données Notion (toutes les minutes) et réagit selon la colonne **Statut** :

1. **⏳ À Générer** : Gemini rédige un post LinkedIn à partir du titre, des notes et des technologies. Le texte est enregistré dans Notion, le statut passe à **✏️ Prêt à Publier**, et un email de notification est envoyé.
2. **🚀 Publier** : le workflow télécharge la photo attachée, publie le post sur LinkedIn, puis passe le statut à **✅ Terminé**.

Entre les deux, tu relis le post, tu ajoutes ta photo, et tu changes le statut toi-même. Rien n'est publié sans ton action.

## Stack

- n8n
- Notion API
- Gemini API (gemini-flash-latest)
- LinkedIn API
- Gmail

## Structure de la base Notion

| Propriété | Type | Rôle |
|---|---|---|
| Titre | Title | Sujet du post |
| Contenu | Text | Notes / idées brutes |
| Technologies | Text ou Multi-select | Outils utilisés |
| Post_Genere | Text | Post écrit par Gemini |
| Ma_Photo | Files & media | Image à publier |
| Statut | Select | `⏳ À Générer`, `✏️ Prêt à Publier`, `🚀 Publier`, `✅ Terminé` |

## Installation

1. Importe `workflow.json` dans n8n (Workflows > Import from file).
2. Crée ta base Notion avec les propriétés ci-dessus et connecte-la au nœud **Surveillance Notion**.
3. Ajoute tes credentials n8n : Notion, Gmail, LinkedIn.
4. Définis la clé Gemini en variable d'environnement (voir `.env.example`) ou colle-la dans le nœud **Gemini Rédige**.
5. Dans le prompt de **Gemini Rédige**, remplace les parties entre crochets `[...]` par ton profil.
6. Remplace l'adresse email du nœud **Mail Notif**.
7. Active le workflow.

## Personnalisation

Tout le style des posts se règle dans le prompt du nœud **Gemini Rédige** : ton, longueur, nombre d'emojis, hashtags, structure.

## Limites connues

Voir [docs/limitations-roadmap.md](docs/limitations-roadmap.md).

## Sécurité

Aucune clé ni credential n'est inclus dans ce dépôt. Ne commit jamais ton fichier `.env`.

## Licence

MIT
