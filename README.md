# 🚀 **SOLUTION-RSYNC : Sauvegarde & Restauration Automatisées** 🛠️

## 🎯 **Objectif du Projet**
Ce projet te permettra de mettre en œuvre une solution complète de **sauvegarde** et **restauration** automatisées à l'aide de **rsync**, en suivant deux stratégies différentes :

- 🏆 **Stratégie 1 : Sauvegarde Incrémentale**
- 🎖️ **Stratégie 2 : Sauvegarde Différentielle**

---

## 📚 **Table des Matières**

- [🔍 Prérequis Logiciels](#-prérequis-logiciels)
- [🛠️ Déployer l'Infrastructure Virtuelle](#️-déployer-linfrastructure-virtuelle)
  - [🖥️ Créer le Serveur de Sauvegarde](#-créer-le-serveur-de-sauvegarde)
  - [💻 Configurer le Serveur de Simulation](#-configurer-le-serveur-de-simulation)
- [🔐 Générer et Échanger les Clés SSH](#-générer-et-échanger-les-clés-ssh)
- [📜 Créer et Rassembler les Livrables](#-créer-et-rassembler-les-livrables)
- [🧪 Tester les Scénarios de Sauvegarde et Restauration](#-tester-les-scénarios-de-sauvegarde-et-restauration)
- [📚 Ressources Utiles](#-ressources-utiles)

---

## 🔍 **Prérequis Logiciels**

Avant de commencer, assure-toi d'avoir :

- **VirtualBox** installé sur ton système.  
  👉 Consulte le dépôt [VIRTUAL-LAB](https://github.com/cyber-dyper/VIRTUAL-LAB) pour apprendre à installer et utiliser VirtualBox.

- Pour ce porjet les versions **serveur** ou **non-bureautique** sont préférables pour éviter de surcharger tes ressources système.  
  👉 Accède aux distributions Linux officielles :  
  - [Debian Server](https://www.debian.org/distrib)  
  - [Ubuntu Server](https://ubuntu.com/download/server)
 
- Les **distributions Linux bureau** si tu préfères pour créer les VMs de test.  
  👉 Toutes les distributions recommandées sont disponibles dans le dépôt [GNU-LINUX](https://github.com/cyber-dyper/GNU-LINUX).

  
- L'accès à un terminal avec `rsync` installé sur chaque machine virtuelle (déjà installé sur la plupart des distributions Linux).

---

## 🛠️ **Déployer l'Infrastructure Virtuelle**

### 🖥️ **Créer le Serveur de Sauvegarde**

1. **Lancer une VM sous Debian ou Ubuntu Server**  

2. **Créer l'arborescence des dossiers de sauvegarde** avec la commande `mkdir` :

```bash
mkdir -p /home/cyberdyper/{emails,fichiers,rh,web,it,vms}/current
```

👉 Cette commande crée automatiquement l'ensemble des dossiers nécessaires pour chaque type de données à sauvegarder.

3. Installer rsync sur le serveur de sauvegarde avec apt :
```bash
sudo apt update && sudo apt install rsync -y
```

👉 Cette commande installe rsync après avoir mis à jour la liste des paquets du système.


### 💻 Configurer le Serveur de Simulation

1. Lancer une VM de simulation sous Debian ou Ubuntu

2. Créer les dossiers de test pour chaque service avec mkdir :
```bash
mkdir -p /home/cyberdyper/{emails/{michel,sonia,joseph},web/{html,css,js,image},it/{client,finance,marketing},fichiers/{ppt,pdf,txt},rh/{bulletins,contrats,conges}}
mkdir /home/cyberdyper/{logs,scripts}
```

✅ Ici, chaque service (ex. : emails, web, IT) dispose de ses propres sous-dossiers pour une meilleure organisation des données.

3. Générer des fichiers de test avec la commande dd :
```bash
dd if=/dev/zero of=/home/cyberdyper/emails/joseph/joseph.pst bs=4096 count=256
```

### 💡 Explication :

`if=/dev/zero` : Remplit le fichier avec des zéros.

`of=` : Spécifie le chemin du fichier à créer.

`bs=4096` : Définit la taille du bloc à 4 Ko.

`count=256` : Crée 256 blocs, soit 256 x 4 Ko = 1 Mo.

## 🔐 Générer et Échanger les Clés SSH

### 🔑 Pourquoi utiliser des clés SSH ?

Les clés SSH permettent de sécuriser la connexion entre les serveurs sans avoir besoin de saisir un mot de passe à chaque connexion.
Cela facilite l'automatisation des sauvegardes avec rsync.

### 🛠️ Étapes pour configurer l'authentification SSH

1. Générer une clé SSH sur le serveur de simulation :
```bash
ssh-keygen -t rsa -b 4096 -N ""
```

👉 Cette commande crée une paire de clés RSA de 4096 bits, sans mot de passe.

2. Copier la clé publique vers le serveur de sauvegarde :
```bash
ssh-copy-id -i ~/.ssh/id_rsa.pub save@srv-backup
```

📤 Cela autorise la connexion SSH sécurisée depuis la machine de simulation vers le serveur de sauvegarde.

3. Vérifier la connexion SSH sans mot de passe :
```bash
ssh save@srv-backup
```

👌 Si tout fonctionne, tu ne devrais pas être invité à saisir un mot de passe.

## 📜 Créer et Rassembler les Livrables

📁 Tous les scripts nécessaires sont disponibles dans les sous-répertoires de ce dépôt SOLUTION-RSYNC.

### 📑 Créer les scripts sur le serveur de simulation

1. Naviguer vers le dossier scripts :
```bash
cd /home/cyberdyper/scripts
```

2. Créer chaque script avec un éditeur de texte (nano, vim, etc.). Nous allons utilser nano ici:
  👉 Tu pourras retourver la liste des commandes shell comme `nano` dans mon autre dépôt [SHELL-COMMANDS](https://github.com/cyber-dyper/SHELL-COMMANDS).
```bash
nano backup_inc.sh
```

3. Tu trouveras la solution des scripts à écrire dans le sous-répertoire correspondant dans ce dépôt (il faut remonter tout en haut).

4. Rendre le script exécutable :
```bash
chmod +x backup_inc.sh
```
### 📋 Répéter pour tous les scripts :

`backup_inc.sh`

`restore_inc.sh`

`backup_dif.sh`

`restore_dif.sh`

## 🧪 **Tester les Scénarios de Sauvegarde et Restauration**

Pour exécuter les scripts de sauvegarde et de restauration, tu as deux options : utiliser la commande `bash` ou exécuter directement le script avec `./`. 

Assure-toi d'abord que tes scripts sont bien exécutables :

```bash
chmod +x /home/cyberdyper/scripts/*.sh
```
### 🌀 Sauvegarde Incrémentale

- Méthode 1 : Avec bash

```bash
bash /home/cyberdyper/scripts/backup_inc.sh
```

- Méthode 2 : En exécutant directement le script

```bash
cd /home/cyberdyper/scripts
./backup_inc.sh
```

### 🔄 Exécuter la Sauvegarde Différentielle

```bash
bash /home/cyberdyper/scripts/backup_dif.sh
```
ou
```bash
./backup_dif.sh
```

### 🛠️ Lancer la Restauration Incrémentale

```bash
bash /home/cyberdyper/scripts/restore_inc.sh all latest
```
ou
```bash
./restore_inc.sh all latest
```

### 💽 Lancer la Restauration Différentielle

```bash
bash /home/cyberdyper/scripts/restore_dif.sh
```
ou
```bash
./restore_dif.sh
```

💡 Astuce : Utiliser la méthode `./script.sh` est particulièrement pratique lorsque tu te trouves déjà dans le dossier des scripts (`cd /home/cyberdyper/scripts`).

## 📚 **Ressources Utiles**

- 🌍 [rsync - Documentation officielle](https://download.samba.org/pub/rsync/rsync.html)
- 💻 [VIRTUAL-LAB - Crée ton laboratoire virtuel](https://github.com/cyber-dyper/VIRTUAL-LAB)
- 📁 [GNU-LINUX - Détails sur les distributions](https://github.com/cyber-dyper/GNU-LINUX)
- 🛠️ [Télécharger Debian Server](https://www.debian.org/distrib)
- 🛠️ [Télécharger Ubuntu Server](https://ubuntu.com/download/server)


✨ Automatise tes sauvegardes comme un pro et dors sur tes deux oreilles ! 🚀😊
