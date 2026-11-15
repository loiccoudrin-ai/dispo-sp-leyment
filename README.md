# DISPO SP Leyment V5 — version connectée à déployer

## Fonctionnalités
- Comptes individuels par e-mail/mot de passe (14 SP à créer par inscription), validation des comptes par un administrateur.
- Déclarations de disponibilité par semaine et période (nuits de semaine / week-end).
- Deux postes par période, sélection uniquement des volontaires disponibles, rotation basée sur le nombre d'affectations sur les 12 dernières semaines.
- Déclaration d'indisponibilité après affectation, appel à volontaires, approbation par administrateur.
- Manœuvres et réponses présent / absent / en attente.
- Gestion des profils et téléphones par l'administrateur.
- Alertes dans l'application et notifications navigateur **uniquement si l'application est ouverte** (actualisation toutes les 45 secondes).

## Installation
1. Créer un projet Supabase sous le contrôle de l'organisation, en région européenne appropriée.
2. Dans SQL Editor, exécuter `schema.sql` **une seule fois** sur une base neuve.
3. Dans Authentication > Providers, activer e-mail et confirmation e-mail. Pour limiter l'accès, préférer l'invitation des membres par un administrateur Supabase plutôt que l'inscription publique.
4. Héberger `index.html` et `manifest.webmanifest` sur un site **HTTPS** (Netlify, Cloudflare Pages ou hébergement institutionnel). Le navigateur doit pouvoir accéder au CDN JS et à Supabase.
5. Au premier lancement, saisir l'URL Supabase et la clé **publique** (anon / publishable). Ne jamais saisir la clé service_role.
6. Créer le premier compte, puis depuis le SQL Editor, remplacer YOUR-USER-UUID dans la commande de fin de `schema.sql` pour le rendre actif et administrateur.
7. Créer les 13 autres comptes, puis les activer dans l'onglet Administration.
8. Activer `pg_cron` si disponible dans Supabase et exécuter la commande `cron.schedule` commentée en fin de `schema.sql` : elle contrôle l'heure locale Europe/Paris et génère le planning à 18 h le jeudi, y compris lors des changements d'heure. Vérifier que l'extension et les permissions de cron sont disponibles sur le projet. Sinon utiliser un ordonnanceur serveur sécurisé qui appelle la fonction SQL.

## Précautions et limites
- **Aucun serveur ni hébergement n'a été créé par ce livrable** : il faut exécuter les étapes ci-dessus pour synchroniser réellement les téléphones.
- **Pas de push en arrière-plan** : prévoir une V5.1 avec Web Push, service worker, abonnements par appareil, et envoi serveur protégé. Les notifications actuelles nécessitent l'application ouverte.
- Les numéros de téléphone sont visibles uniquement aux utilisateurs actifs, et les modifications de profils sont réservées aux administrateurs. Valider la conformité RGPD et les règles internes du SDIS avant de saisir des données réelles.
- Les membres peuvent modifier leur disponibilité après génération : une indisponibilité d'un SP affecté doit être déclarée depuis le bouton « Indisponible » du planning pour déclencher la procédure de remplacement. Le changement seul de disponibilité ne modifie pas rétroactivement l'affectation.
- Les postes vacants ne sont pas attribués automatiquement après publication ; intervention manuelle requise. Les remplacements sont proposés puis validés.
- Le planning de 2 SP ne garantit pas la capacité réelle de départ, les qualifications ni la couverture opérationnelle. Ne remplace jamais l'outil officiel d'alerte et de disponibilité du SDIS.
- L'équité est calculée sur les affectations, pas sur le temps réel passé en permanence. Les mêmes personnes peuvent couvrir nuits et week-end si elles sont disponibles.
- La PWA est installable selon le navigateur ; pour une installation mobile fiable, ajouter des icônes PNG 192/512 et un service worker de cache si souhaité.

## Tests recommandés avant usage réel
- Inscription et validation de 14 comptes, contrôle des droits utilisateurs.
- Simulation d'un jeudi 18 h et passage heure d'été/hiver.
- Moins de 2 SP disponibles : poste vacant visible.
- Refus d'un SP, proposition d'un volontaire, approbation administrateur.
- Accès aux données depuis deux téléphones et vérification du rafraîchissement.
- Test des notifications navigateur application ouverte.
