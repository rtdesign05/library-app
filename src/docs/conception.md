# GREEN UP ACADEMY - Application de Gestion de Bibliothèque en Ligne

**Développement Full-Stack Avancé**  
**CHAPITRE 1 – Architecture Full-Stack et Responsabilités**

**Mini-Projet 1 – Conception architecturale d'une application**

---

## Document de Conception Architecturale

**Projet :** Système de Gestion de Bibliothèque en Ligne  
**Groupe :** Raoult & AUBIERGE  
**Date :** 30 janvier 2026  
**Version :** 1.0

---

## Table des matières

1. [Introduction](#introduction)
2. [Analyse des besoins](#analyse-des-besoins)
3. [Architecture technique](#architecture-technique)
4. [Modélisation de la base de données](#modélisation-de-la-base-de-données)
5. [Architecture logicielle](#architecture-logicielle)
6. [Plan de développement](#plan-de-développement)

---

## 1. Introduction

### 1.1 Contexte du projet

Ce document présente l'architecture complète d'une application web de gestion de bibliothèque en ligne. L'application vise à moderniser la gestion des emprunts et à faciliter l'accès au catalogue pour les lecteurs tout en offrant aux administrateurs des outils de gestion efficaces.

### 1.2 Objectifs

- Digitaliser le processus de gestion des emprunts
- Offrir un accès 24/7 au catalogue en ligne
- Automatiser le suivi des emprunts et des retours
- Fournir une interface intuitive pour les lecteurs et les administrateurs
- Garantir la sécurité des données utilisateurs

### 1.3 Portée du projet

**Inclus dans ce projet :**

- Authentification sécurisée (lecteurs et administrateurs)
- Gestion complète du catalogue (CRUD)
- Système d'emprunt et de retour
- Historique des emprunts
- Recherche et filtrage de livres

**Hors périmètre (évolutions futures) :**

- Système de réservation
- Notifications par email
- Paiement des amendes en ligne
- Système de recommandations

### 1.4 Stack technologique

| Couche               | Technologie             | Version | Justification                                                                        |
| -------------------- | ----------------------- | ------- | ------------------------------------------------------------------------------------ |
| **Frontend**         | Next.js                 | 14.x    | Framework React moderne avec SSR/SSG, routing intégré, optimisations automatiques    |
| **Backend**          | Next.js API Routes      | 14.x    | API intégrée, développement full-stack unifié, déploiement simplifié                 |
| **Base de données**  | MongoDB                 | 7.x     | Flexibilité du schéma, scalabilité horizontale, adaptation aux données documentaires |
| **ORM**              | Mongoose                | 8.x     | Modélisation des données, validation, relations                                      |
| **Authentification** | NextAuth.js             | 4.x     | Solution complète pour Next.js, support multi-providers                              |
| **UI Library**       | Tailwind CSS            | 3.x     | Productivité, composants réutilisables, responsive design                            |
| **Validation**       | Zod                     | 3.x     | Validation TypeScript-first, type-safe                                               |
| **State Management** | React Context / Zustand | -       | Gestion d'état simple et performante                                                 |

**Avantages de cette stack :**

- Développement full-stack unifié (JavaScript/TypeScript)
- Courbe d'apprentissage optimisée
- Performances élevées (SSR, optimisations automatiques)
- Écosystème riche et communauté active
- Scalabilité horizontale (MongoDB, Next.js)
- Déploiement simplifié (Vercel, Railway)

---

## 2. Analyse des besoins

### 2.1 Besoins fonctionnels

#### 2.1.1 User Stories

**En tant que Lecteur :**

| ID  | User Story                                                                                                                        | Priorité | Critères d'acceptation                                                                                                                     |
| --- | --------------------------------------------------------------------------------------------------------------------------------- | -------- | ------------------------------------------------------------------------------------------------------------------------------------------ |
| US1 | En tant que **lecteur**, je veux **consulter le catalogue de livres** pour découvrir les ouvrages disponibles                     | Haute    | - Affichage de tous les livres<br>- Informations visibles : titre, auteur, disponibilité<br>- Pagination fonctionnelle                     |
| US2 | En tant que **lecteur**, je veux **rechercher un livre par titre, auteur ou catégorie** pour trouver rapidement ce que je cherche | Haute    | - Barre de recherche fonctionnelle<br>- Résultats en temps réel<br>- Filtres par catégorie                                                 |
| US3 | En tant que **lecteur**, je veux **emprunter un livre disponible** pour l'ajouter à mon historique personnel                      | Haute    | - Bouton "Emprunter" visible si disponible<br>- Vérification de la disponibilité<br>- Confirmation de l'emprunt<br>- Mise à jour du statut |
| US4 | En tant que **lecteur**, je veux **retourner un livre emprunté** pour le rendre disponible aux autres                             | Haute    | - Bouton "Retourner" dans mon historique<br>- Confirmation du retour<br>- Mise à jour automatique de la disponibilité                      |
| US5 | En tant que **lecteur**, je veux **consulter mon historique d'emprunts** pour suivre mes lectures                                 | Moyenne  | - Liste de tous mes emprunts<br>- Statut visible (en cours, retourné, en retard)<br>- Dates d'emprunt et de retour                         |
| US6 | En tant que **lecteur**, je veux **voir les détails d'un livre** pour obtenir plus d'informations                                 | Moyenne  | - Page dédiée pour chaque livre<br>- Affichage : description, ISBN, catégorie, etc.                                                        |

**En tant qu'Administrateur :**

| ID   | User Story                                                                                                | Priorité | Critères d'acceptation                                                                                 |
| ---- | --------------------------------------------------------------------------------------------------------- | -------- | ------------------------------------------------------------------------------------------------------ |
| US7  | En tant qu'**administrateur**, je veux **ajouter un nouveau livre** au catalogue pour enrichir l'offre    | Haute    | - Formulaire de création<br>- Validation des champs<br>- Message de confirmation                       |
| US8  | En tant qu'**administrateur**, je veux **modifier les informations d'un livre** pour corriger les erreurs | Haute    | - Formulaire pré-rempli<br>- Sauvegarde des modifications<br>- Validation des données                  |
| US9  | En tant qu'**administrateur**, je veux **supprimer un livre** pour retirer les ouvrages obsolètes         | Moyenne  | - Bouton de suppression<br>- Confirmation avant suppression<br>- Vérification (pas d'emprunt en cours) |
| US10 | En tant qu'**administrateur**, je veux **voir tous les emprunts en cours** pour suivre l'activité         | Moyenne  | - Dashboard avec statistiques<br>- Liste des emprunts actifs<br>- Emprunts en retard mis en évidence   |

**En tant qu'Utilisateur (Général) :**

| ID   | User Story                                                                               | Priorité | Critères d'acceptation                                                                                |
| ---- | ---------------------------------------------------------------------------------------- | -------- | ----------------------------------------------------------------------------------------------------- |
| US11 | En tant qu'**utilisateur**, je veux **m'inscrire** pour créer un compte                  | Haute    | - Formulaire d'inscription<br>- Validation email unique<br>- Mot de passe sécurisé (min 8 caractères) |
| US12 | En tant qu'**utilisateur**, je veux **me connecter** pour accéder à mon espace personnel | Haute    | - Formulaire de connexion<br>- Session sécurisée (JWT/cookies)<br>- Redirection après connexion       |
| US13 | En tant qu'**utilisateur**, je veux **me déconnecter** pour sécuriser mon compte         | Moyenne  | - Bouton de déconnexion<br>- Suppression de la session                                                |

### 2.2 Besoins non-fonctionnels

| Catégorie         | Exigence                | Spécification                                                   |
| ----------------- | ----------------------- | --------------------------------------------------------------- |
| **Performance**   | Temps de chargement     | < 2 secondes pour toutes les pages                              |
| **Sécurité**      | Authentification        | JWT avec refresh tokens, HTTPS obligatoire                      |
| **Sécurité**      | Mots de passe           | Hachage bcrypt (12 rounds minimum)                              |
| **Sécurité**      | Protection              | CORS configuré, rate limiting sur l'API, validation des entrées |
| **Disponibilité** | Uptime                  | 99% minimum                                                     |
| **Scalabilité**   | Utilisateurs simultanés | Support de 100+ utilisateurs                                    |
| **Accessibilité** | Standards               | Respect WCAG 2.1 niveau AA                                      |
| **Responsive**    | Appareils               | Desktop, tablette, mobile (breakpoints standards)               |
| **Navigateurs**   | Support                 | Chrome, Firefox, Safari, Edge (2 dernières versions)            |

### 2.3 Contraintes techniques

- Utilisation obligatoire de Next.js et MongoDB
- Hébergement sur plateforme gratuite (Vercel, Railway)
- Respect des bonnes pratiques de sécurité OWASP
- Code versionné sur Git avec commits structurés
- Documentation complète en Markdown

---

## 3. Architecture technique

### 3.1 Architecture globale

```
┌─────────────────────────────────────────────────────────────┐
│                    COUCHE PRÉSENTATION                       │
│  ┌────────────────────────────────────────────────────────┐ │
│  │  Next.js Frontend (React Components + Pages)           │ │
│  │  - UI Components (Tailwind CSS)                        │ │
│  │  - Pages (SSR/CSR)                                     │ │
│  │  - Client State (Context/Zustand)                      │ │
│  └────────────────────────────────────────────────────────┘ │
└─────────────────────────────────────────────────────────────┘
                            ▼ HTTP/HTTPS
┌─────────────────────────────────────────────────────────────┐
│                    COUCHE APPLICATION                        │
│  ┌────────────────────────────────────────────────────────┐ │
│  │  Next.js API Routes                                    │ │
│  │  - Authentication (NextAuth.js)                        │ │
│  │  - Business Logic                                      │ │
│  │  - Validation (Zod)                                    │ │
│  │  - Error Handling                                      │ │
│  └────────────────────────────────────────────────────────┘ │
└─────────────────────────────────────────────────────────────┘
                            ▼ Mongoose ODM
┌─────────────────────────────────────────────────────────────┐
│                    COUCHE DONNÉES                            │
│  ┌────────────────────────────────────────────────────────┐ │
│  │  MongoDB Database                                       │ │
│  │  - Collections : users, books, borrows                 │ │
│  │  - Indexes & Constraints                               │ │
│  └────────────────────────────────────────────────────────┘ │
└─────────────────────────────────────────────────────────────┘
```

### 3.2 Architecture en couches

**1. Couche Présentation (Frontend)**

- **Responsabilité :** Interface utilisateur, interactions, affichage
- **Technologies :** Next.js, React, Tailwind CSS
- **Composants principaux :**
  - Pages (Home, Catalogue, Mon compte, Dashboard admin)
  - Composants UI réutilisables (BookCard, SearchBar, Modal)
  - Layouts (Header, Footer, Sidebar)
  - Formulaires (Login, Register, BookForm)

**2. Couche Application (Backend)**

- **Responsabilité :** Logique métier, validation, authentification
- **Technologies :** Next.js API Routes, NextAuth.js, Zod
- **Modules :**
  - Authentification et autorisation
  - Controllers (logique métier)
  - Middlewares (validation, auth)
  - Services (business logic réutilisable)

**3. Couche Données (Database)**

- **Responsabilité :** Persistance, requêtes, intégrité
- **Technologies :** MongoDB, Mongoose
- **Composants :**
  - Modèles Mongoose (User, Book, Borrow)
  - Schémas de validation
  - Indexes pour optimisation

### 3.3 Structure de fichiers du projet

```
library-management/
├── public/                     # Assets statiques
│   ├── images/
│   └── favicon.ico
├── src/
│   ├── app/                    # Next.js App Router
│   │   ├── (auth)/
│   │   │   ├── login/
│   │   │   └── register/
│   │   ├── (user)/
│   │   │   ├── books/
│   │   │   ├── my-borrows/
│   │   │   └── profile/
│   │   ├── (admin)/
│   │   │   ├── dashboard/
│   │   │   └── manage-books/
│   │   ├── api/                # API Routes
│   │   │   ├── auth/
│   │   │   │   ├── register/route.ts
│   │   │   │   └── [...nextauth]/route.ts
│   │   │   ├── books/
│   │   │   │   ├── route.ts
│   │   │   │   └── [id]/route.ts
│   │   │   └── borrows/
│   │   │       ├── route.ts
│   │   │       └── [id]/
│   │   ├── layout.tsx
│   │   └── page.tsx
│   ├── components/             # Composants React
│   │   ├── auth/
│   │   │   ├── LoginForm.tsx
│   │   │   └── RegisterForm.tsx
│   │   ├── books/
│   │   │   ├── BookCard.tsx
│   │   │   ├── BookList.tsx
│   │   │   ├── BookForm.tsx
│   │   │   └── SearchBar.tsx
│   │   ├── borrows/
│   │   │   └── BorrowHistory.tsx
│   │   ├── layout/
│   │   │   ├── Header.tsx
│   │   │   ├── Footer.tsx
│   │   │   └── Sidebar.tsx
│   │   └── ui/                 # Composants UI réutilisables
│   │       ├── Button.tsx
│   │       ├── Input.tsx
│   │       ├── Modal.tsx
│   │       └── Card.tsx
│   ├── lib/                    # Utilitaires et configurations
│   │   ├── mongodb.ts          # Connexion MongoDB
│   │   ├── auth.ts             # Configuration NextAuth
│   │   └── validations.ts      # Schémas Zod
│   ├── models/                 # Modèles Mongoose
│   │   ├── User.ts
│   │   ├── Book.ts
│   │   └── Borrow.ts
│   ├── services/               # Logique métier
│   │   ├── bookService.ts
│   │   ├── borrowService.ts
│   │   └── userService.ts
│   ├── middleware/             # Middlewares personnalisés
│   │   ├── auth.ts
│   │   └── errorHandler.ts
│   ├── types/                  # Types TypeScript
│   │   └── index.ts
│   └── utils/                  # Fonctions utilitaires
│       ├── constants.ts
│       └── helpers.ts
├── .env.local                  # Variables d'environnement
├── .env.example
├── .gitignore
├── next.config.js
├── package.json
├── tailwind.config.ts
├── tsconfig.json
└── README.md
```

## 4. Modélisation de la base de données

### 4.1 Schéma Conceptuel (MCD)

```
┌──────────────┐           ┌──────────────┐           ┌──────────────┐
│     USER     │           │    BORROW    │           │     BOOK     │
├──────────────┤           ├──────────────┤           ├──────────────┤
│ _id          │──────1:N──│ user         │──────N:1──│ _id          │
│ name         │           │ book         │           │ title        │
│ email        │           │ borrowDate   │           │ author       │
│ password     │           │ dueDate      │           │ isbn         │
│ role         │           │ returnDate   │           │ category     │
│ createdAt    │           │ status       │           │ available... │
│ updatedAt    │           │ createdAt    │           │ createdAt    │
└──────────────┘           └──────────────┘           └──────────────┘
```

### 4.2 Relations et cardinalités

| Relation      | Type        | Description                                             |
| ------------- | ----------- | ------------------------------------------------------- |
| User → Borrow | One-to-Many | Un utilisateur peut avoir plusieurs emprunts            |
| Book → Borrow | One-to-Many | Un livre peut être emprunté plusieurs fois (historique) |
| Borrow → User | Many-to-One | Chaque emprunt appartient à un seul utilisateur         |
| Borrow → Book | Many-to-One | Chaque emprunt concerne un seul livre                   |

---

## 5. Architecture logicielle

### 5.1 Flux de données détaillé

#### Scénario 1 : Emprunter un livre

```
[Utilisateur]
    │
    │ 1. Clique sur "Emprunter" (BookCard)
    ▼
[Frontend - BookCard.tsx]
    │
    │ 2. Appel API : POST /api/borrows
    ▼
[API Route - /api/borrows/route.ts]
    │
    │ 3. Vérification de l'authentification (middleware)
    ▼
[Middleware - auth.ts]
    │
    │ 4. Validation du payload (Zod)
    ▼
[Validation - validations.ts]
    │
    │ 5. Logique métier (vérifier disponibilité)
    ▼
[Service - borrowService.ts]
    │
    │ 6a. Vérifier si le livre est disponible
    ▼
[Modèle Book]
    │
    │ 6b. Créer l'emprunt
    ▼
[Modèle Borrow]
    │
    │ 6c. Décrémenter availableCopies
    ▼
[Modèle Book]
    │
    │ 7. Réponse JSON
    ▼
[Frontend]
    │
    │ 8. Mise à jour de l'interface (toast de confirmation)
    ▼
[Utilisateur]
```

#### Scénario 2 : Connexion utilisateur

```
[Utilisateur]
    │
    │ 1. Remplit le formulaire (LoginForm)
    ▼
[Frontend - LoginForm.tsx]
    │
    │ 2. signIn() NextAuth
    ▼
[NextAuth - [...nextauth]/route.ts]
    │
    │ 3. Credentials Provider
    ▼
[Auth Config - lib/auth.ts]
    │
    │ 4. Recherche utilisateur par email
    ▼
[Modèle User]
    │
    │ 5. Comparaison du mot de passe (bcrypt)
    ▼
[Méthode User.comparePassword()]
    │
    │ 6. Génération de la session JWT
    ▼
[NextAuth]
    │
    │ 7. Cookie sécurisé (httpOnly)
    ▼
[Frontend]
    │
    │ 8. Redirection vers Dashboard
    ▼
[Utilisateur]
```

### 5.2 Diagramme de séquence - Création de livre (Admin)

```
Admin         Frontend          API Route       Validation      Service         DB
  │               │                 │                │              │            │
  │  Remplit      │                 │                │              │            │
  │  formulaire   │                 │                │              │            │
  ├──────────────>│                 │                │              │            │
  │               │ POST /api/books │                │              │            │
  │               ├────────────────>│                │              │            │
  │               │                 │ Vérifier auth  │              │            │
  │               │                 │ (isAdmin?)     │              │            │
  │               │                 ├───────────────>│              │            │
  │               │                 │                │ Valider      │            │
  │               │                 │                │ données      │            │
  │               │                 │                ├─────────────>│            │
  │               │                 │                │              │ book.save()│
  │               │                 │                │              ├───────────>│
  │               │                 │                │              │<───────────┤
  │               │                 │<───────────────┴──────────────┤            │
  │               │<────────────────┤ 201 Created                                │
  │<──────────────┤ Livre ajouté                                                 │
  │               │                                                               │
```

### 5.3 Séparation des préoccupations (SoC)

| Couche            | Responsabilité                      | Ne doit PAS                     |
| ----------------- | ----------------------------------- | ------------------------------- |
| **Composants UI** | Affichage, interactions utilisateur | Logique métier, accès DB direct |
| **API Routes**    | Routing, orchestration              | Logique métier complexe         |
| **Services**      | Logique métier                      | Accès DB direct, affichage      |
| **Modèles**       | Schéma, validation, requêtes DB     | Logique métier                  |
| **Middlewares**   | Auth, validation, logging           | Logique métier                  |

---

## 6. Plan de développement

### 6.1 Méthodologie Agile

**Organisation :**

- Sprints de 1 semaine
- Daily stand-ups (asynchrones)
- Retrospectives hebdomadaires
- Kanban board (GitHub Projects / Trello)

### 6.2 Répartition des tâches (3 semaines - 60h)

#### **Sprint 1 : Backend & Base de données (Step 1 - 20h)**

#### **Sprint 2 : Frontend & Intégration (Step 2 - 20h)**

#### **Sprint 3 : Tests, Déploiement & Documentation (Step 3 - 20h)**

### 6.3 Outils de suivi

- **Gestion de projet :** GitHub Projects
- **Communication :** Discord / Whattsapp
- **Versionning :** Git + GitHub
- **Tests API :** Postman / Thunder Client
- **CI/CD :** Vercel (déploiement auto sur push)

### 6.4 Conventions de code

**Git Commits :**

- `feat:` nouvelle fonctionnalité
- `fix:` correction de bug
- `docs:` documentation
- `style:` formatage code
- `refactor:` refactorisation
- `test:` ajout de tests

**Branches :**

- `main` : production
- `develop` : développement
- `feature/nom-feature` : nouvelles fonctionnalités

---

**Signatures :**

Raoult : **\*\***\_\_\_\_**\*\***  
AUBIERGE : **\*\***\_\_\_\_**\*\***
