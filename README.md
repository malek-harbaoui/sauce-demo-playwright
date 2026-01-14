# 🧪 Tests Automatisés SauceDemo - Playwright

Projet de tests automatisés pour le site [SauceDemo](https://www.saucedemo.com) utilisant Playwright et JavaScript.

## 📋 Structure du Projet

```
saucedemo-playwright-tests/
├── tests/
│   ├── helpers/
│   │   └── testHelpers.js       # Fonctions réutilisables
│   ├── micro-tests.spec.js      # Tests unitaires de chaque fonction
│   └── e2e-purchase.spec.js     # Test end-to-end complet
├── testData.json                # Données de test
├── playwright.config.js         # Configuration Playwright
├── package.json
└── README.md
```

## 🚀 Installation

1. **Installer les dépendances :**
```bash
npm install
```

2. **Installer les navigateurs Playwright :**
```bash
npx playwright install
```

3. **Créer le dossier screenshots (optionnel) :**
```bash
mkdir screenshots
```
> Le dossier screenshots sera créé automatiquement lors du premier échec de test

## 🧩 Fonctionnalités

### Micro Tests (tests/micro-tests.spec.js)
12 tests indépendants qui peuvent être exécutés séparément :
- **MT01** : Connexion utilisateur
- **MT02** : Vérification d'un produit
- **MT03** : Vérification de tous les produits
- **MT04** : Ajout d'un produit au panier
- **MT05** : Navigation vers le panier
- **MT06** : Vérification du produit dans le panier
- **MT07** : Navigation vers checkout
- **MT08** : Remplissage du formulaire de checkout
- **MT09** : Vérification du récapitulatif de commande
- **MT10** : Finalisation de la commande
- **MT11** : Vérification du message de confirmation
- **MT12** : Vérification disparition du badge panier

### Test E2E (tests/e2e-purchase.spec.js)
Un test complet qui couvre tout le parcours d'achat :
1. Connexion avec l'utilisateur standard
2. Vérification de tous les produits (nom, prix, image, bouton)
3. Ajout d'un produit au panier
4. Consultation du panier
5. Processus de checkout
6. Remplissage du formulaire
7. Vérification du récapitulatif (quantité, prix total = prix produit + taxe)
8. Finalisation de la commande
9. Vérification du message "Thank you for your order!"
10. Vérification que le badge du panier a disparu

## ▶️ Exécution des Tests

### Exécuter tous les tests
```bash
npm test
```

### Exécuter uniquement les micro tests
```bash
npm run test:micro
```

### Exécuter uniquement le test E2E
```bash
npm run test:e2e
```

### Exécuter en mode headed (voir le navigateur)
```bash
npm run test:headed
```

### Exécuter en mode UI (interface interactive)
```bash
npm run test:ui
```

## 📊 Rapports

### Rapport HTML (Playwright)
Après l'exécution des tests, générer et ouvrir le rapport :
```bash
npm run report
```

### Rapport Allure
1. **Générer le rapport Allure :**
```bash
npm run allure:generate
```

2. **Ouvrir le rapport Allure :**
```bash
npm run allure:open
```

3. **Ou servir directement :**
```bash
npm run allure:serve
```

## 📝 Données de Test

Les données sont stockées dans `testData.json` :
- **Utilisateur** : `standard_user` / `secret_sauce`
- **Informations checkout** : Test User, 12345
- **6 produits** avec leurs détails (nom, prix, image)

## ✨ Fonctionnalités Clés

✅ **Logs détaillés** : Logs de succès (✅) et d'échec (❌) pour chaque étape
✅ **Screenshots automatiques** : Capture d'écran complète en cas d'échec avec timestamp
✅ **Commentaires complets** : Toutes les fonctions sont documentées avec JSDoc
✅ **Utilisation de data-test** : Sélecteurs fiables via data-test (configuration dans playwright.config.js)
✅ **Assertions complètes** : Vérifications à chaque étape avec gestion d'erreurs
✅ **Code modulaire** : Fonctions réutilisables dans différents scénarios
✅ **Try-catch partout** : Gestion d'erreurs robuste avec logs et screenshots
✅ **Traces en cas d'échec** : Conservation des traces pour débogage
✅ **Rapports multiples** : HTML et Allure pour différents besoins
✅ **Hook beforeAll** : Connexion optimisée dans le contexte E2E

## 🎯 Cas d'Usage

### Réutiliser les fonctions dans un nouveau test

```javascript
const { test } = require('@playwright/test');
const testData = require('../testData.json');
const { login, addProductToCart, goToCart } = require('./helpers/testHelpers');

test('Mon nouveau test', async ({ page }) => {
  await login(page, testData.user.username, testData.user.password);
  await addProductToCart(page, testData.products[1].name);
  await goToCart(page);
  // ... suite du test
});
```

## 📌 Configuration

Le fichier `playwright.config.js` est configuré avec :
- **baseURL** : https://www.saucedemo.com
- **headless** : true (mode sans interface par défaut)
- **testIdAttribute** : 'data-test'
- **Screenshots** : uniquement en cas d'échec
- **Traces** : conservées en cas d'échec
- **Rapports** : HTML + Allure

## 🔍 Vérifications Effectuées

### Sur les produits :
- Nom du produit
- Prix du produit
- Image du produit (src contient le chemin)
- Bouton "Add to cart" visible

### Dans le panier :
- Nom du produit
- Prix du produit
- Quantité du produit

### Au checkout :
- Quantité correcte
- Calcul du total : Prix produit + Taxe = Total
- Message de confirmation
- Disparition du badge du panier

## 🎨 Logs Console

Les tests affichent des logs détaillés avec des emojis pour faciliter le suivi :
- 🔐 Connexion
- 🔍 Vérification
- ✅ Succès
- 🛒 Panier
- 💰 Prix/Montants
- 📝 Formulaire
- ➡️ Navigation

## 📦 Dépendances

- `@playwright/test` : Framework de test
- `allure-playwright` : Reporter Allure
- `allure-commandline` : CLI Allure pour générer les rapports

---

**Développé avec ❤️ pour l'automatisation de tests**
