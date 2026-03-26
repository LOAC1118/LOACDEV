# CRM Commandes — Kit White-Label
## Guide de déploiement rapide

---

## 📦 Contenu du kit

| Fichier | Rôle |
|---|---|
| `admin.html` | Back-office : configuration société, catalogue, remises, charte |
| `commande.html` | Bon de commande revendeur (interface client) |
| `config.json` | Base de données partagée (générée automatiquement) |

---

## 🚀 Mise en route pour un nouveau client

### Étape 1 — Ouvrir l'administration
Ouvrez `admin.html` dans un navigateur moderne (Chrome, Firefox, Edge).

### Étape 2 — Configurer la société
- Menu **Société** : renseignez le nom, logo (URL), email de réception des commandes
- Menu **Collection** : nom de la collection, date limite, délai de livraison

### Étape 3 — Configurer la charte graphique
- Menu **Charte graphique** : ajustez les 4 couleurs pour correspondre à l'identité visuelle du client

### Étape 4 — Importer le catalogue
Deux options :
- **CSV** (menu Import → onglet CSV) : fichier avec colonnes `categorie;reference;nom;prix_ht;tva;colisage`
- **JSON** (menu Import → onglet JSON) : tableau d'objets avec les mêmes propriétés
- Ou saisie manuelle produit par produit (menu Catalogue)

### Étape 5 — Configurer les remises
- Menu **Profils & remises** : créez autant de profils que nécessaire (ex: Biocoop 15%, Indépendant 10%, Standard 0%)

### Étape 6 — Sauvegarder et exporter
1. Cliquez **Sauvegarder** (la config est stockée localement dans le navigateur)
2. Cliquez **Exporter config.json** pour télécharger le fichier de configuration
3. Placez ce `config.json` dans le même dossier que `commande.html`

### Étape 7 — Partager le bon de commande
Partagez le fichier `commande.html` avec les revendeurs :
- Par email en pièce jointe
- Via un intranet ou serveur web
- Sur une clé USB ou dropbox partagée

---

## 🔧 Fonctionnement technique

### Partage de configuration
La configuration circule de deux manières :
1. **localStorage** (navigation locale) : l'admin sauvegarde dans le navigateur, le BDC lit depuis le même navigateur
2. **config.json** (déploiement) : exportez le fichier, placez-le à côté du BDC — le BDC le charge automatiquement au premier accès

### Envoi des commandes
Les bons de commande sont envoyés via **FormSubmit.co** (service gratuit, sans backend) :
- Pas de code serveur requis
- L'email de destination est celui configuré dans Société → Email de réception
- Une copie est envoyée automatiquement au revendeur
- ⚠️ FormSubmit demande une confirmation email au premier envoi sur une nouvelle adresse

### Sécurité
- L'admin (`admin.html`) peut être protégé par un accès réseau restreint
- Les revendeurs n'ont accès qu'à `commande.html` (pas de mot de passe nécessaire côté revendeur)
- Pour une protection admin, renommez le fichier ou hébergez sur un réseau privé

---

## 📋 Format d'import CSV

```csv
categorie;reference;nom;prix_ht;tva;colisage
Céréales;BIO-001;Farine T65;2.50;5.5;12
Biscuits;BIS-001;Sablés nature;3.20;5.5;6
Huiles;HUI-001;Huile de tournesol;4.80;5.5;6
```

**Séparateur :** `;` (point-virgule) ou `,` (virgule), détecté automatiquement  
**Encodage :** UTF-8 recommandé  
**TVA acceptée :** 5.5, 10 ou 20

---

## 📄 Format d'import JSON

```json
[
  { "categorie": "Céréales", "reference": "BIO-001", "nom": "Farine T65", "prix_ht": 2.50, "tva": 5.5, "colisage": 12 },
  { "categorie": "Biscuits", "reference": "BIS-001", "nom": "Sablés nature", "prix_ht": 3.20, "tva": 5.5, "colisage": 6 }
]
```

---

## ❓ Dépannage

**Le BDC affiche "Catalogue non disponible"**  
→ Les 3 fichiers ne sont pas dans le même dossier, ou le `config.json` n'a pas été exporté depuis l'admin.

**Les emails ne sont pas reçus**  
→ Vérifiez que l'adresse email dans Société est correcte. FormSubmit envoie un email de confirmation au premier usage — consultez les spams.

**Le logo ne s'affiche pas**  
→ L'URL du logo doit être une adresse publique (https://...). Les images locales (C:\...) ne fonctionnent pas.

**Les couleurs ne s'appliquent pas sur le BDC**  
→ Sauvegardez dans l'admin, puis exportez le `config.json` et remplacez-le à côté de `commande.html`.

---

*Kit développé pour une utilisation white-label — remplacez simplement le contenu de config.json pour chaque nouveau client.*
