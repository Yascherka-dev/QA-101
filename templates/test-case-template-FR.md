# Template de Cas de Test
## Conforme ISTQB

**Standard ISTQB:** Ce template suit les standards ISTQB Foundation Level pour la documentation de cas de test.

## Informations de Base

**ID Cas de Test:** `TC-XXX-XXX`  
**Nom du Cas de Test:** [Nom bref et descriptif]  
**Module/Fonctionnalité:** [ex: Authentification, Gestion Utilisateurs]  
**Créé Par:** [Votre nom]  
**Date de Création:** [Date]  
**Dernière Mise à Jour:** [Date]  
**Version:** [1.0]

## Détails du Test

**Priorité:** [Élevée / Moyenne / Faible]  
**Type de Test:** [Fonctionnel / Non-Fonctionnel / Régression / Fumée]  
**Niveau de Test (ISTQB):** [Composant/Unité / Intégration / Système / Acceptation]  
**Temps Estimé:** [ex: 5 minutes]

## Préconditions

[Liste des conditions qui doivent être remplies avant l'exécution du test]

**Exemple:**
- Compte utilisateur existe avec email: test@example.com
- Application accessible à https://app.example.com
- Utilisateur n'est pas actuellement connecté
- Navigateur: Chrome (dernière version)

## Données de Test

[Spécifier les données de test nécessaires]

**Exemple:**
- Email Valide: user@example.com
- Mot de Passe Valide: Test123!
- Email Invalide: invalid@test.com
- Mot de Passe Invalide: wrongpass

## Étapes de Test

| Étape # | Action | Résultat Attendu |
|--------|--------|-----------------|
| 1 | Naviguer vers la page de connexion | Page de connexion affichée |
| 2 | Entrer l'email dans le champ email | Email entré correctement |
| 3 | Entrer le mot de passe dans le champ mot de passe | Mot de passe entré (masqué) |
| 4 | Cliquer sur le bouton "Connexion" | Système traite la demande de connexion |
| 5 | Vérifier la redirection | Utilisateur redirigé vers le tableau de bord |
| 6 | Vérifier le message de bienvenue | "Bienvenue, [Nom Utilisateur]" affiché |

## Résultat Attendu

[Résultat global attendu]

**Exemple:**
L'utilisateur se connecte avec succès et est redirigé vers le tableau de bord avec un message de bienvenue personnalisé.

## Postconditions

[État après l'exécution du test]

**Exemple:**
- Utilisateur est connecté
- Session est active
- Utilisateur est sur la page du tableau de bord

## Résultat Réel

[À remplir pendant l'exécution]

## Statut du Test

- [ ] Non Exécuté
- [ ] Passé
- [ ] Échoué
- [ ] Bloqué
- [ ] Ignoré

## ID Défaut

[Si le test échoue, lier au rapport de bug]

**ID Défaut:** [ex: BUG-123]

## Notes/Commentaires

[Observations supplémentaires, cas limites, ou actions de suivi]

---

## Exemple: Cas de Test Complet

**ID Cas de Test:** `TC-AUTH-001`  
**Nom du Cas de Test:** Connexion Utilisateur avec Identifiants Valides  
**Module/Fonctionnalité:** Authentification  
**Priorité:** Élevée  
**Type de Test:** Fonctionnel  
**Niveau de Test:** Système

**Préconditions:**
- Compte utilisateur existe: testuser@example.com / Password123!
- Application accessible
- Utilisateur est déconnecté

**Étapes de Test:**

| Étape # | Action | Résultat Attendu |
|--------|--------|-----------------|
| 1 | Ouvrir le navigateur et naviguer vers https://app.example.com/login | Page de connexion charge avec succès |
| 2 | Vérifier que le formulaire de connexion est affiché | Champs email et mot de passe visibles |
| 3 | Entrer "testuser@example.com" dans le champ email | Email entré correctement |
| 4 | Entrer "Password123!" dans le champ mot de passe | Mot de passe entré (affiché comme points) |
| 5 | Cliquer sur le bouton "Connexion" | Indicateur de chargement apparaît brièvement |
| 6 | Attendre la redirection | Utilisateur redirigé vers /dashboard |
| 7 | Vérifier que l'URL contient "/dashboard" | URL est correcte |
| 8 | Vérifier que le message de bienvenue est affiché | Message "Bienvenue, Test User" visible |
| 9 | Vérifier que le menu utilisateur affiche l'utilisateur connecté | Menu utilisateur affiche "Test User" |

**Résultat Attendu:**  
L'utilisateur se connecte avec succès et est redirigé vers le tableau de bord avec message de bienvenue personnalisé et menu utilisateur mis à jour.

**Résultat Réel:**  
[À remplir pendant l'exécution]

**Statut du Test:** [ ] Passé [ ] Échoué [ ] Bloqué

**ID Défaut:** [Si applicable]

**Notes:**  
- Test exécuté sur Chrome 120.0
- Temps de réponse: 1.2 secondes
- Aucun problème observé

