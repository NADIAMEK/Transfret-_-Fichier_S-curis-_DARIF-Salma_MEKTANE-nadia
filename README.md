Transfert de fichiers sécurisé

// Projet réalisé par DARIF Salma et MEKTANE Nadia.

 1- Description du Projet

Ce projet consiste en la conception et l’implémentation d’un système Client–Serveur sécurisé permettant le transfert de fichiers via TCP.
L’application intègre des mécanismes de sécurité (chiffrement), un protocole de session, ainsi qu’un système de contrôle d’intégrité, garantissant que les données échangées sont confidentielles, intactes et protégées.

2- Objectifs du Projet

Concevoir un protocole de communication structuré en trois phases (Authentification, Négociation, Transfert).

Sécuriser le transfert des fichiers via :

- chiffrement symétrique AES

- vérification d’intégrité SHA-256.

- Implémenter un serveur TCP multi-threadé, capable de gérer plusieurs clients simultanément.

- Mettre en place un client console capable de préparer, chiffrer et envoyer un fichier en suivant le protocole défini.

3- Architecture du Système

Le système se compose de deux modules principaux :

- SecureFileServer — Serveur TCP multi-threadé

- SecureFileClient — Client console interactif

Les deux applications échangent via un protocole applicatif organisé en trois phases sécurisées.

 1.1 SecureFileServer (Serveur)
1.1.1 Écoute et Gestion des Connexions

Le serveur écoute sur un port TCP défini.

Chaque nouveau client est immédiatement pris en charge par un thread dédié :
ClientTransferHandler

Le serveur peut donc gérer plusieurs clients en parallèle.

1.1.2 Protocole de Session
Phase 1 — Authentification

Le serveur reçoit :

- login

- mot de passe

- Les identifiants valides sont stockés dans :

une Map<String, String> statique, ou un fichier externe.

Réponses possibles :

-  AUTH_OK

-  AUTH_FAIL → la connexion est fermée.

Phase 2 — Négociation

Le serveur reçoit les méta-données du fichier :

- nom

- taille (en octets)

- hachage SHA-256

Si les données sont valides :
 réponse : READY_FOR_TRANSFER

Phase 3 — Transfert et Vérification

Le serveur :

- reçoit le fichier chiffré,

- le déchiffre (AES),

- enregistre le fichier,

- recalcule le SHA-256,

- compare avec celui envoyé par le client.

Réponses possibles :

-  TRANSFER_SUCCESS

- TRANSFER_FAIL

 2.2 SecureFileClient (Client)
2.2..1 Interface Utilisateur

Le client fonctionne en ligne de commande.
L’utilisateur saisit :

- IP du serveur

- port

- login

- mot de passe

- chemin du fichier à envoyer

2.2 Pré-traitement du Fichier

Avant l’envoi, le client doit :

 1. Calculer le hachage SHA-256

Utilisation de l’API :
java.security.MessageDigest

 2. Chiffrer le fichier en AES

Utilisation de :
javax.crypto
Algorithme et mode :
AES/ECB/PKCS5Padding

La clé AES est partagée entre le client et le serveur (méthode d’échange laissée à l’appréciation du développeur).

2.3 Communication

Le client suit strictement les trois phases imposées par le protocole :

1 - AUTH
2 -  NEGOTIATION
3 -  TRANSFER


Chaque étape nécessite la validation explicite du serveur.

4- Sécurité et Cryptographie
AES (Confidentialité)

Algorithme : AES

Mode : ECB avec padding PKCS5

Utilisation : chiffrement complet du contenu binaire du fichier

Note : Le mode ECB n’est utilisé ici qu’à des fins pédagogiques.

SHA-256 (Intégrité)

Utilisation de l’API Java MessageDigest

L’empreinte est calculée côté client puis vérifiée côté serveur

Si les deux hachages sont identiques → fichier intact.

 5- Structure du Répertoire
/src
    ├── server/...
    ├── client/...
    └── utils/...
/received_files
/out
.gitignore
README.md

6-  Exécution
1. Lancer le Serveur

Importer le projet dans IntelliJ 
Exécuter la classe SecureFileServer.

2. Lancer le Client

Exécuter la classe SecureFileClient
Saisir les informations demandées dans le terminal.
