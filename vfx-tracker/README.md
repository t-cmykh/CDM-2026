# VFX Tracker

Outil de suivi de production pour un projet VFX découpé entre **deux superviseurs CG**. Une seule page HTML autonome (aucune installation, aucun serveur requis) pour recenser tous les shots et assets, leurs tâches par département, le brief, les références et les échanges liés à chaque tâche.

## Lancer l'outil

Aucune compilation nécessaire. Deux façons de l'utiliser :

- **En local** : ouvrez `vfx-tracker/index.html` directement dans un navigateur.
- **Hébergé** (recommandé pour un usage à deux) : servez le dossier `vfx-tracker/` via GitHub Pages, un serveur interne au studio, ou tout hébergement statique, puis partagez l'URL aux deux superviseurs.

## Ce que l'outil couvre

- **Shots** et **Assets** comme deux entités séparées, chacune avec ses propres départements (configurables dans les paramètres ⚙️) :
  - Shots : Layout, Animation, Crowd, FX, Sim Cloth/Hair, Lighting, Comp, Matte Painting, Roto/Prep, CFX
  - Assets : Modeling, Sculpt, Rig, Surfacing/LookDev, Groom, FX Setup, Texturing
- **Statuts** de pipeline : À faire, En cours, Retake, En revue, Approuvé, Omit — plus un indicateur **Bloqué** (avec raison) indépendant du statut.
- **Brief**, **références** (liens multiples avec libellé) et un **fil de communication** (notes horodatées et signées) par tâche.
- Assignation à l'un des deux superviseurs CG (noms éditables dans l'en-tête) + artiste assigné, priorité, échéance, frame range.
- Trois vues : **Dashboard** (KPI, répartition par département/superviseur/statut, retards), **Board** kanban (glisser-déposer entre statuts), **Table** triable/filtrable.
- Filtres combinables : type, séquence/catégorie, département, superviseur, statut, priorité, tâches bloquées.
- Sélecteur **« Vous êtes »** en haut à droite : identifie qui écrit une note, par poste — pas un vrai compte, juste une signature locale.

## Partage des données entre les deux superviseurs

L'outil est un fichier statique, sans base de données serveur. Les données vivent dans le `localStorage` du navigateur de chaque poste. Pour que les deux superviseurs travaillent sur les mêmes données :

1. **Export / Import (le plus simple)** — Bouton *Export* pour télécharger un JSON, *Import* pour le charger sur l'autre poste. L'import propose une **fusion intelligente** (garde par tâche la version la plus récente via `updatedAt`) ou un remplacement complet.
2. **Synchronisation distante (optionnelle)** — Dans ⚙️ Paramètres, renseignez une URL de stockage JSON (ex. un Worker Cloudflare + KV, un bin jsonbin.io, un petit serveur maison) et utilisez *Pousser* (PUT) / *Récupérer* (GET, avec la même fusion par `updatedAt`). Un jeton `Authorization` optionnel est supporté.

Aucune de ces options n'est du temps réel : c'est un flux **pull/push volontaire**, pensé pour deux personnes qui synchronisent explicitement (avant/après une session de travail, par exemple).

## Notes techniques

- Aucune dépendance externe, aucun appel réseau par défaut (fonctionne hors-ligne / sur réseau studio fermé) — seule la synchronisation distante optionnelle fait un `fetch` vers l'URL que vous configurez.
- Toutes les données sont stockées en clair dans le `localStorage` du navigateur (`vfx_tracker_data_v1`). Pensez à exporter régulièrement si vous videz le cache du navigateur.
- Bouton « Charger des exemples » sur l'écran vide pour découvrir l'outil avec des données de démonstration.
