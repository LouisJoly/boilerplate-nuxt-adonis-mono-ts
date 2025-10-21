# Boilerplate Monorepo Full-Stack TypeScript

> Architecture moderne et évolutive pour applications web full-stack avec partage de types entre frontend et backend

[![Node.js](https://img.shields.io/badge/Node.js-18+-339933?logo=node.js&logoColor=white)](https://nodejs.org/)
[![pnpm](https://img.shields.io/badge/pnpm-8+-F69220?logo=pnpm&logoColor=white)](https://pnpm.io/)
[![TypeScript](https://img.shields.io/badge/TypeScript-5.0+-3178C6?logo=typescript&logoColor=white)](https://www.typescriptlang.org/)

## 🏗️ Vue d'Ensemble de l'Architecture

Ce projet est structuré en monorepo utilisant pnpm Workspaces pour gérer les dépendances partagées et les applications de manière optimisée.

```
boilerplate-nuxt-adonis-mono-ts/
├── apps/
│   ├── front/        # Application Nuxt 3 (Frontend)
│   └── back/         # API AdonisJS 6 (Backend)
├── packages/
│   └── types/        # Types et interfaces TypeScript partagés
├── .gitignore
├── package.json      # Configuration racine
├── pnpm-lock.yaml
├── pnpm-workspace.yaml
└── tsconfig.json     # Configuration TypeScript globale
```

### Fonctionnement du Monorepo

- **pnpm Workspaces** : Permet de gérer les dépendances de manière centralisée et optimisée
- **Partage de code** : Les types partagés sont définis dans `packages/types` et peuvent être importés dans n'importe quelle application
- **Isolation** : Chaque application (front/back) a sa propre configuration et ses propres dépendances

## 🚀 Prérequis

Avant de commencer, assurez-vous d'avoir installé :

- [Node.js](https://nodejs.org/) (version 18 ou supérieure)
- [pnpm](https://pnpm.io/) (version 8 ou supérieure)

## 🛠️ Installation

1. **Cloner le dépôt**
   ```bash
   git clone https://github.com/LouisJoly/boilerplate-nuxt-adonis-mono-ts.git
   cd boilerplate-nuxt-adonis-mono-ts
   ```

2. **Installer les dépendances**
   ```bash
   pnpm install
   ```

## 🛠️ Vérification et Construction des Packages

Avant de démarrer le développement, assurez-vous que les packages front et back peuvent être construits sans erreur. Cela évite les problèmes lors du démarrage en mode développement.

### Construire le frontend (Nuxt 3)
```bash
pnpm --filter front build
```

### Construire le backend (AdonisJS 6)
```bash
pnpm --filter back build
```

Si une construction échoue, vérifiez les erreurs de dépendances ou de configuration avant de continuer.

##  Structure Détaillée

### Répertoire Racine
- `apps/` - Contient les applications frontend et backend
- `packages/` - Contient les packages partagés (types, utilitaires, etc.)
- `pnpm-workspace.yaml` - Configuration des workspaces pnpm
- `tsconfig.json` - Configuration TypeScript globale

### Applications

#### Frontend (Nuxt 3)
```
apps/front/
├── public/          # Fichiers statiques
├── server/          # Configuration serveur Nuxt
├── src/
│   ├── assets/      # Fichiers statics (images, polices, etc.)
│   ├── components/  # Composants Vue
│   ├── composables/ # Composable functions
│   ├── layouts/     # Layouts de l'application
│   ├── pages/       # Pages de l'application
│   └── types/       # Types spécifiques au frontend
├── .nuxt/          # Généré par Nuxt (ne pas modifier)
├── nuxt.config.ts   # Configuration Nuxt
└── package.json     # Dépendances du frontend
```

#### Backend (AdonisJS 6)
```
apps/back/
├── app/
│   ├── controllers/ # Contrôleurs
│   ├── models/      # Modèles de données
│   └── validators/  # Validateurs de requêtes
├── config/          # Fichiers de configuration
├── database/
│   └── migrations/  # Migrations de base de données
├── public/          # Fichiers statiques
├── start/
│   └── routes.ts    # Définition des routes
├── .env             # Variables d'environnement
├── .adonisrc.json   # Configuration AdonisJS
└── package.json     # Dépendances du backend
```

### Packages Partagés

#### Types Partagés
```
packages/types/
├── src/
│   ├── api/         # Types d'API partagés
│   ├── domain/      # Entités du domaine
│   └── index.ts     # Export des types
├── package.json     # Configuration du package
└── tsconfig.json    # Configuration TypeScript
```

## 🧩 Guide de Développement

### Ajouter de nouveaux types partagés

1. **Créer un nouveau type** dans le package `@monorepo/types` :
   ```typescript
   // packages/types/src/domain/user.ts
   export interface User {
     id: string;
     email: string;
     name: string;
     createdAt: Date;
   }
   ```

2. **Exporter le type** dans le fichier d'index :
   ```typescript
   // packages/types/src/index.ts
   export * from './domain/user';
   ```

3. **Utiliser le type dans le frontend** :
   ```typescript
   import { User } from '@monorepo/types';
   
   const user: User = {
     id: '1',
     email: 'user@example.com',
     name: 'John Doe',
     createdAt: new Date()
   };
   ```

4. **Utiliser le type dans le backend** :
   ```typescript
   import { User } from '@monorepo/types';
   
   class UserController {
     public async store({ request }: HttpContextContract) {
       const userData: Omit<User, 'id' | 'createdAt'> = request.body();
       // ...
     }
   }
   ```

### Bonnes Pratiques

- **Types partagés** : Placez tous les types partagés entre le frontend et le backend dans `packages/types`
- **Dépendances** : Utilisez `pnpm add -w` pour ajouter des dépendances au workspace racine
- **Scripts** : Définissez des scripts dans les `package.json` de chaque application pour les tâches courantes

## 📄 Licence

Ce projet est sous licence MIT - voir le fichier [LICENSE](LICENSE) pour plus de détails.