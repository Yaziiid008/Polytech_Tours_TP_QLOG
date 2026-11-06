# Rapport de revue des spécifications — Corpo Padel

## 1. Introduction

Ce document présente la revue des spécifications de l'application Corpo Padel, utilisée pour gérer des tournois de padel entre entreprises.

L'objectif est d'identifier les exigences fonctionnelles, techniques et de sécurité, de relever les ambiguïtés du cahier des charges et de préparer les cas de test.

**Référence :** https://www.pividori.fr/polytech/specifications/

## 2. Exigences fonctionnelles

L'application comporte trois rôles :

- **Visiteur :** accès à la page d'accueil uniquement.
- **Joueur :** consultation des matchs, résultats, planning et gestion de son profil.
- **Administrateur :** gestion des événements, matchs, joueurs, équipes et comptes.

### Règles métier

| ID | Exigence | Résultat attendu |
|---|---|---|
| R1 | Date de création | Impossible de créer un événement ou match avec une date passée. |
| R2 | Conflit de piste | Deux matchs ne peuvent pas occuper la même piste au même créneau. |
| R3 | Unicité des équipes | Une équipe ne peut jouer qu'un seul match dans un événement. |
| R4 | Suppression | Seuls les événements ou matchs A_VENIR peuvent être supprimés. |
| R5 | Score | Les scores doivent respecter les règles du tennis. |
| R6 | Filtrage | Le joueur voit par défaut uniquement les événements et matchs le concernant. |
| R7 | Classement | Victoire = 3 points, défaite = 0 point ; départage par sets. |

## 3. Exigences techniques et de sécurité

### Exigences techniques

- Frontend : Vue 3, Vite et Tailwind CSS.
- Backend : Python et FastAPI.
- Base de données : SQLite.
- Communication : API REST avec préfixe /api/v1.
- Authentification : JWT.

### Exigences de sécurité

- SEC-01 : les mots de passe sont hachés avec bcrypt.
- SEC-02 : les tokens JWT expirent après 24 heures.
- SEC-03 : les routes réservées aux administrateurs sont protégées côté serveur.
- SEC-04 : un compte est bloqué pendant 30 minutes après 5 échecs de connexion.
- SEC-05 : les données sont validées côté serveur.
- SEC-06 : les entrées utilisateur ne doivent pas permettre d'injection ou de XSS.
- SEC-07 : les photos de profil doivent être au format JPG, JPEG ou PNG, avec une taille maximale de 2 Mo.

## 4. Ambiguïtés et points à clarifier

| ID | Exigence | Question / ambiguïté |
|---|---|---|
| AMB-01 | R1 | Quel fuseau horaire définit la date du jour ? |
| AMB-02 | R2 | Comment gérer deux matchs dont les horaires se chevauchent ? |
| AMB-03 | R3 | Comment réagir si une même équipe est inscrite deux fois dans un événement ? |
| AMB-04 | R4 | Un événement A_VENIR contenant un match terminé peut-il être supprimé ? |
| AMB-05 | R5 | Combien de sets sont nécessaires pour gagner un match ? |
| AMB-06 | R5 | Comment représenter le résultat d'un tie-break ? |
| AMB-07 | R7 | Comment départager deux entreprises à égalité de points et de sets ? |
| AMB-08 | SEC-04 | Le compteur des tentatives est-il associé à l'adresse email ou au compte utilisateur ? |
| AMB-09 | SEC-07 | Comment vérifier le type réel d'une image si son extension est falsifiée ? |
| AMB-10 | Profil | Quelles restrictions s'appliquent aux modifications de l'email ? |

Ces points sont des questions de clarification, pas des anomalies confirmées de l'application.

## 5. Matrice de traçabilité

La matrice associe chaque exigence à au moins un cas de test prévu pour la phase de recette.

| Exigence | Cas de test | Scénario |
|---|---|---|
| R1 | TC-001 | Créer un match avec une date passée. |
| R2 | TC-002 | Créer deux matchs sur la même piste au même créneau. |
| R3 | TC-003 | Inscrire une équipe à deux matchs d'un même événement. |
| R4 | TC-004 | Essayer de supprimer un match terminé. |
| R5 | TC-005 | Saisir un score invalide comme 6-6. |
| R6 | TC-006 | Vérifier le filtre par défaut d'un joueur. |
| R7 | TC-007 | Contrôler le calcul du classement. |
| SEC-01 | TC-008 | Vérifier qu'aucun mot de passe n'est enregistré en clair. |
| SEC-02 | TC-009 | Vérifier le refus d'un JWT expiré. |
| SEC-03 | TC-010 | Appeler une route admin avec un compte joueur. |
| SEC-04 | TC-011 | Effectuer cinq connexions incorrectes. |
| SEC-05 | TC-012 | Envoyer des données invalides directement à l'API. |
| SEC-06 | TC-013 | Vérifier l'absence d'exécution de contenu XSS. |
| SEC-07 | TC-014 | Envoyer une photo de plus de 2 Mo. |

Ces scénarios sont prévisionnels. Les prérequis, étapes détaillées, données d'entrée et résultats attendus seront développés dans le cahier de recette de la phase 3.

## 6. Recette interne et recette externe

### Recette interne

Elle vérifie que le logiciel respecte les spécifications établies.

Exemple : vérifier qu'un match terminé ne peut pas être supprimé.

### Recette externe

Elle vérifie que le logiciel répond réellement aux besoins des utilisateurs.

Exemple : vérifier qu'un joueur peut facilement retrouver ses prochains matchs.

## 7. Conclusion

La revue des spécifications a permis d'identifier les principales règles fonctionnelles et exigences de sécurité.

Plusieurs points nécessitent une clarification, notamment la gestion des horaires, les règles de score, les égalités de classement et le blocage des comptes.

La matrice de traçabilité constitue une base pour préparer le plan de test et le cahier de recette.

Aucun défaut d'implémentation n'est considéré comme confirmé à cette étape. Les anomalies éventuelles seront documentées après exécution des tests manuels.
