# Fiche Version - Grist Gantt Widget

## 📌 1. Version Actuelle : v0.3.1
* **Date :** 21 Juin 2026
* **État :** Stable (avec mode Démo autonome)

## 🛠️ 2. Stack Technique
* **Frontend :** HTML5, CSS3 natif (sans CDN ni framework externe)
* **API Externe :** Grist Plugin API (`grist-plugin-api.js`)
* **Langage :** JavaScript (Vanilla ES6, encapsulé dans une IIFE en mode strict)

## 🚀 3. Fonctionnalités Prêtes et Fonctionnelles
* **Gestion des Dates (Header) :** Génération dynamique de l'en-tête temporel par mois et par année calculée à partir des dates minimales et maximales du projet.
* **Intégration Grist complète :** 
  * Mapping flexible des colonnes (`notion`, `notionTxt`, `dateDebut`, `dateFin`, `etat`, `etatTxt`).
  * Auto-détection et gestion des structures de données complexes de type Référence (`Ref`) pour les colonnes Notion et État sans casser le modèle Grist.
  * Synchronisation bidirectionnelle : la sélection d'une barre déplace le curseur dans l'interface de Grist.
* **Interactions en Drag & Drop :** 
  * Déplacement complet d'une barre de Gantt pour modifier les dates.
  * Redimensionnement précis de la durée par poignées latérales (gauche et droite).
  * Enregistrement asynchrone des deltas de jours (`BulkUpdateRecord`) directement dans Grist.
* **Fenêtre d'édition (Modale) :** 
  * Formulaire d'édition au double-clic (Notion, Date début, Date fin, État).
  * Barre de recherche dynamique intégrée dans la modale pour filtrer les notions disponibles.
  * Verrouillage automatique de la saisie textuelle si la colonne Grist d'origine est une référence structurelle (`Ref`).
* **UI/UX & Rendu visuel :**
  * Info-bulles (Tooltips) dynamiques affichant au survol le nom, l'état, les dates exactes et la durée calculée en jours.
  * Légende en pied de page avec code couleur par état (Découverte, Approfondissement, Révision, Évaluation).
  * Système de notifications discrètes (Toasts) pour confirmer les succès ou afficher les erreurs d'enregistrement.
  * Boutons de contrôle dans l'en-tête pour rafraîchir les données et afficher/masquer la colonne des notions.
* **Mode Démo :** Exécution autonome fonctionnelle avec données fictives injectées si l'API Grist n'est pas détectée.

## ⚠️ 4. Bugs Connus / Limitations Actuelles
* La modification du nom d'une notion est bloquée par design lorsque la colonne Grist est de type `Ref` afin de préserver l'intégrité de la structure.
* Pour que Grist identifie l'ID d'une référence d'État, au moins une ligne de la table doit déjà utiliser cet état.

## 📐 5. Règles Architecturales Strictes
* Tout le code (Structure, Styles et Logique JavaScript) doit être condensé et distribué dans un fichier unique `index.html` pour faciliter son import en tant que Custom Widget dans Grist.
* Pas de dépendance CSS ou JS lourde (Pas de jQuery, Tailwind ou React), utilisation exclusive du JS natif et du CSS interne.
* Préservation de l'architecture découplée : les fonctions de Drag & Drop n'altèrent que les dates et n'envoient pas de requêtes de modification sur l'État ou la Notion sauf validation explicite par la modale.