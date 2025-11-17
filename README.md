Transfert de fichiers sécurisé

///Projet réalisé par DARIF Salma et MEKTANE Nadia

Ce projet met en place un système Client–Serveur sécurisé pour le transfert de fichiers via TCP, avec chiffrement AES et contrôle d’intégrité SHA-256.

1. Description du Projet

Le système permet :

- La transmission sécurisée de fichiers entre un client et un serveur.

- L’authentification des utilisateurs avant tout transfert.

- La vérification d’intégrité des fichiers grâce au SHA-256.

- Le chiffrement des fichiers via AES pour garantir la confidentialité.

- La gestion de plusieurs clients simultanément (multi-threading côté serveur).

2. Objectifs

  1.Implémenter un protocole en trois phases :

- Authentification

- Négociation des métadonnées du fichier

- Transfert et vérification du fichier

  2.Sécuriser les transferts avec :

- AES (128 bits, ECB/PKCS5Padding)

- SHA-256 pour garantir l’intégrité

 3.Créer un serveur TCP capable de gérer plusieurs clients simultanément.

 4.Créer un client console interactif pour préparer et envoyer un fichier.

3. Architecture du Système

Le projet se compose de deux modules principaux :

3.1 SecureFileServer (serveur)

- Écoute sur un port TCP défini (par défaut 9000).

- Chaque connexion est prise en charge par un thread dédié (ClientTransferHandler).

- Gère le protocole en trois phases :

     1. Authentification : login + mot de passe → AUTH_OK / AUTH_FAIL.

     2. Négociation : envoie des métadonnées du fichier → READY_FOR_TRANSFER.

     3.Transfert : réception du fichier chiffré, déchiffrement AES, vérification SHA-256 → TRANSFER_SUCCESS / TRANSFER_FAIL.

3.2 SecureFileClient (client)

- Interface console pour saisir :

     . IP du serveur

     . Port

     . Login et mot de passe

     . Chemin du fichier à envoyer

- Pré-traitement du fichier :

     . Calcul SHA-256

     . Chiffrement AES

     . Respect strict du protocole : Auth → Négociation → Transfert

4. Sécurité et Cryptographie
- AES (Confidentialité)

     . Clé partagée entre client et serveur (128 bits, ECB/PKCS5Padding).

     . Chiffre le contenu complet du fichier.

- SHA-256 (Intégrité)

    . Calculé côté client et vérifié côté serveur.

    . Permet de détecter toute altération du fichier pendant le transfert.


5. Structure du Répertoire
/src
 └── securefile
      ├── SecureFileServer.java       # Classe principale du serveur
   
      ├── SecureFileClient.java       # Classe principale du client
   
      ├── ClientTransferHandler.java  # Gestionnaire de connexion côté serveur
   
      └── CryptoUtils.java            # Fonctions utilitaires de cryptographie
   

/received_files                        # Dossier créé automatiquement pour stocker les fichiers reçus

/out                                    # Dossier de compilation/output (si nécessaire)

/.gitignore                             # Fichier pour ignorer certains fichiers dans Git

README.md                               # Documentation du projet

6. Exécution
6.1 Lancer le serveur

Importer le projet dans IntelliJ.

Exécuter la classe SecureFileServer.

Le serveur écoute sur le port défini (9000 par défaut).

6.2 Lancer le client

Exécuter la classe SecureFileClient.

Saisir les informations demandées : IP, port, login, mot de passe, chemin du fichier.

Le client chiffre le fichier, l’envoie au serveur, et affiche le résultat (TRANSFER_SUCCESS ou TRANSFER_FAIL).
