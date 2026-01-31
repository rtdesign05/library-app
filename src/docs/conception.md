# GREEN UP ACADEMY

**Développement Full-Stack Avancé**  
**CHAPITRE 1 – Architecture Full-Stack et Responsabilités**

**Mini-Projet 1 – Conception architecturale d'une application**

## Document de Conception Architecturale

**Application de Gestion de Bibliothèque en Ligne**

**Groupe :** Raoult & OBIERGE  
**Date :** 30 janvier 2026

### Introduction

Ce document décrit l'architecture full-stack d'une application de gestion de bibliothèque en ligne. L'application permet aux lecteurs de consulter le catalogue, d'emprunter et de retourner des livres, et aux administrateurs de gérer les ouvrages.

**Stack technologique retenue**

- **Front-end** : Next.js (basé sur React) – pour une interface réactive, un rendu serveur possible et une excellente UX
- **Back-end** : API Routes intégrées à Next.js – pour la logique métier, l'authentification et la validation
- **Base de données** : MongoDB (NoSQL) – pour sa flexibilité de schéma, sa scalabilité et son adaptation aux données documentaires

**Avantages de cette stack**

- Développement full-stack unifié (même langage, même projet)
- Vision globale du projet et autonomie accrue
- Prototypage rapide
- Respect du principe de séparation des préoccupations (SoC) et de l'architecture en couches

### 1. Fonctionnalités principales (User Stories) – Tâche 7

- En tant que **lecteur**, je veux **consulter le catalogue** de livres pour découvrir les ouvrages disponibles
- En tant que **lecteur**, je veux **emprunter un livre** disponible pour l'ajouter à mon historique personnel
- En tant que **lecteur**, je veux **retourner un livre** emprunté pour le rendre disponible aux autres
- En tant qu’**administrateur**, je veux **ajouter, modifier et supprimer** un livre pour gérer le catalogue
- En tant qu’**utilisateur**, je veux **m’inscrire et me connecter** pour accéder aux fonctionnalités personnalisées

### 2. Entités de la base de données et relations – Tâche 8

**Modèle MongoDB (NoSQL)**

- **Livre**
  - title : String (requis)
  - author : String (requis)
  - isbn : String (unique, requis)
  - category : String
  - availableCopies : Number (défaut : 1)

- **Utilisateur**
  - name : String
  - email : String (unique, requis)
  - password : String (hashé)
  - role : Enum ["reader", "admin"]

- **Emprunt**
  - user : Reference → Utilisateur
  - book : Reference → Livre
  - borrowDate : Date (défaut : now)
  - dueDate : Date
  - returnDate : Date
  - status : Enum ["borrowed", "returned", "overdue"]

**Relations**

- Un utilisateur peut avoir plusieurs emprunts (One-to-Many)
- Un livre peut être concerné par plusieurs emprunts (One-to-Many)

### 3. Endpoints API nécessaires – Tâche 9

- **Authentification**
  - POST /api/auth/register
  - POST /api/auth/login

- **Livres**
  - GET /api/books (liste + recherche)
  - GET /api/books/:id (détail)
  - POST /api/books (création – admin)
  - PUT /api/books/:id (modification – admin)
  - DELETE /api/books/:id (suppression – admin)

- **Emprunts**
  - POST /api/borrows (emprunter – vérifie disponibilité)
  - PUT /api/borrows/:id/return (retourner)
  - GET /api/borrows/my (historique personnel)

### 4. Schéma d’architecture et flux de données – Tâche 10

**Flux de communication typique** (inspiré du schéma 3.1 du cours)
