
# Plan d'Évolution - Grist Gantt Widget

## 🎯 Objectif : Passage de la v0.3.1 à la v0.4.0 (Pluriannuel, Congés & Échelles de zoom)

Ce document décrit les fonctionnalités et les règles métier pour la version 0.4. L'implémentation doit se faire dans le fichier unique `gantt-widget.html` sans ajouter de bibliothèque externe.

---

## 🚀 1. Nouvelles Fonctionnalités à Implémenter

### A. Affichage Pluriannuel et Années Scolaires Restreintes

* **Concept :** Remplacer le calcul automatique strict basé sur les tâches par un découpage figé en 1, 2 ou 3 années scolaires consécutives, affichées côte à côte.
* **Règles temporelles strictes :** * Une année scolaire commence le **1er septembre** et s'achève le **30 juin**.
* **Exclusion de l'été :** Les mois de juillet et août ne sont pas rendus visuellement (gain d'espace d'environ 16%).


* **Contrôles UI :** Ajouter un `<select>` dans la `.header-bar` avec les options `1 an`, `2 ans` et `3 ans`.
* **Persistance :** Le choix de l'utilisateur est enregistré via `grist.setOption('yearsScope', value)`.

### B. Échelles de Temps (Zoom) & Magnétisme

* **Niveaux de détail :** Ajouter un sélecteur d'échelle (`Jour`, `Semaine`, `Quinzaine`) dans la `.header-bar`.
* **Magnétisme (Snapping) optionnel :** * Un bouton Toggle (ex: `🧲 Magnétisme : ON/OFF`) permet de forcer l'alignement des tâches sur le début de la semaine ou de la quinzaine lors du Drag & Drop.
* **Raccourci clavier (Bypass) :** Maintenir la touche `Shift` lors du relâchement de la souris (`onMouseUp`) désactive temporairement le magnétisme pour un ajustement fin au jour près.


* **Persistance :** L'état du magnétisme et du zoom est sauvegardé via les options Grist.

### C. Visualisation des Congés Scolaires

* **Rendu Visuel :** Les congés apparaissent en arrière-plan (z-index inférieur aux tâches) sous forme de colonnes verticales (gris foncé `#555` semi-transparent).
* **Source de données (Multi-tables) :** Les dates sont récupérées depuis une seconde table Grist nommée **`Vacances`** (colonnes attendues : `Nom`, `Date_Debut`, `Date_Fin`).

---

## 🛠️ 2. Évolutions Techniques de l'Architecture

1. **Mathématiques de la Timeline :** * Réécriture de la fonction `dateToPct` (ou similaire) pour convertir les dates en pixels tout en ignorant l'existence de juillet et août.
* Modification de `buildHeader` pour sauter de juin à septembre et potentiellement afficher des repères de semaines.


2. **Variable d'Échelle de Rendu :** Remplacer les valeurs fixes (ex: `CELL_WIDTH_DAYS = 30`) par une variable dynamique `pxPerDay` pilotée par le niveau de zoom choisi (Jour/Semaine/Quinzaine).
3. **Appels API Grist :** Utiliser `grist.docApi.fetchTable('Vacances')` en parallèle de l'écoute des records principaux pour charger les congés avant le rendu du Gantt.

---

## ⚠️ 3. Contraintes de Développement & Sécurité

* **Zéro régression du Drag & Drop :** Le déplacement et le redimensionnement doivent fonctionner parfaitement malgré les "sauts" temporels de l'été.
* **Sécurité des Saisies (Bouclier Estival) :** * Le formulaire de la modale doit rejeter (avec Toast d'erreur) toute tentative d'enregistrement d'une date tombant en juillet ou en août.
* Si un Drag & Drop se termine dans la période estivale, la tâche rebondit à sa position initiale sans altérer la base de données.


* **Intégrité du mode Démo :** Générer des données fictives pour les tâches ET pour les congés (Toussaint, Noël, etc.) afin de pouvoir tester l'interface hors connexion.
* **Fichier Unique :** Conserver l'intégralité du code (HTML, CSS natif, JS vanilla) dans le fichier unique. Aucune dépendance externe.