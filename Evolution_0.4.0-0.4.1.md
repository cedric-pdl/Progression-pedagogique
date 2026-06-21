# Plan d'Évolution - Grist Gantt Widget
## 🎯 Objectif : Passage de la v0.4.0 à la v0.4.1 (Restauration des Fonctionnalités Critiques & Alignement Grist)

Ce document fournit des instructions ultra-détaillées et impératives destinées à une IA pour corriger la version 0.4.0 et générer la version 0.4.1. L'objectif est de réintégrer l'ensemble des fonctionnalités ergonomiques, analytiques et sécuritaires de la version 0.3m qui ont été malencontreusement supprimées lors de la refonte du moteur temporel scolaire, sans casser ce dernier.

---

## 🛠️ 1. Restauration des 6 Piliers Fonctionnels Perdus

### Pilier A : Déclaration et Mappage des Colonnes Grist (Panneau Latéral)
* **Instruction :** Tu dois réintroduire la configuration complète du schéma Grist lors de l'appel à `grist.ready`. Sans cela, le panneau de droite de Grist reste vide.
* **Spécifications techniques :**
  ```javascript
  grist.ready({
      requiredAccess: "full",
      columns: [
          { name: "notion", type: "Any", title: "Colonne Notion (Réf ou Texte)" },
          { name: "notionTxt", type: "Text", title: "Texte de la Notion (Formule, optionnel)", optional: true },
          { name: "dateDebut", type: "Date", title: "Colonne Date de début" },
          { name: "dateFin", type: "Date", title: "Colonne Date de fin" },
          { name: "etat", type: "Any", title: "Colonne État (Réf ou Texte)", optional: true },
          { name: "etatTxt", type: "Text", title: "Texte de l'État (Formule, optionnel)", optional: true }
      ]
  });