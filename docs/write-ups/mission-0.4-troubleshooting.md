# Mission 0.4 — Troubleshooting Linux de base

## Méthodologie

Dans cette mission, j’ai appliqué une méthode simple de diagnostic en plusieurs étapes.
D’abord, j’ai observé le symptôme exact pour comprendre ce qui ne fonctionnait pas. Ensuite, j’ai isolé la zone du problème : permissions, service, paquet, commande ou réseau. Après cela, j’ai formulé une hypothèse sur la cause probable, puis je l’ai testée avec des commandes adaptées. Enfin, j’ai corrigé le problème avec la solution minimale nécessaire.

## Panne 1 — Permissions fichier

### Symptôme

Je n’arrivais plus à lire le fichier `config.txt` avec la commande `cat`. Le système renvoyait un message de type *Permission denied*.

### Diagnostic

J’ai vérifié les permissions du fichier avec `ls -la`. J’ai constaté que le fichier était en mode `000`, donc sans aucun droit pour le propriétaire, le groupe ou les autres utilisateurs.

### Cause racine

Le fichier avait été volontairement cassé avec `chmod 000`, ce qui supprimait toute possibilité de lecture, d’écriture ou d’exécution.

### Correction

J’ai restauré des permissions adaptées avec `chmod 600`. Cela permet au propriétaire de lire et modifier le fichier, tout en empêchant les autres utilisateurs d’y accéder.

## Panne 2 — Service arrêté

### Symptôme

La connexion SSH vers la VM ne fonctionnait plus. Depuis l’hôte, la tentative de connexion retournait un message indiquant que la connexion au port 22 était refusée.

### Diagnostic

Le service SSH et son socket avaient été arrêtés. Sans eux, le serveur n’écoutait plus sur le port 22, donc aucune connexion distante n’était possible.

### Cause racine

Le problème venait de l’arrêt du service `ssh` et de `ssh.socket`. En plus, si le service n’est pas activé au démarrage, il peut rester indisponible après un reboot.

### Correction

J’ai utilisé la console locale de la VM pour redémarrer `ssh` et `ssh.socket`, puis j’ai activé SSH au démarrage avec `systemctl enable ssh`. Après cela, la connexion SSH a de nouveau fonctionné normalement.

## Panne 3 — Paquet manquant

### Symptôme

La commande `nmap localhost` ne fonctionnait pas. Le système indiquait que la commande était introuvable.

### Diagnostic

Le message d’erreur montrait que `nmap` n’était pas installé sur le système. Ce n’était donc pas un problème de PATH ou de permissions, mais simplement l’absence du paquet.

### Cause racine

L’outil `nmap` n’était pas présent sur l’installation Ubuntu Server de base.

### Correction

J’ai installé le paquet avec `sudo apt install nmap -y`. Après installation, la commande a fonctionné et m’a montré que le port 22 était ouvert sur la machine.

## Panne 4 — Commande introuvable

### Symptôme

La commande `python --version` ne fonctionnait pas, alors que `python3 --version` répondait correctement.

### Diagnostic

J’ai vérifié que Python 3 était bien installé, mais que la commande `python` n’existait pas. Cela signifiait qu’il manquait le lien de compatibilité entre `python` et `python3`.

### Cause racine

Sur Ubuntu moderne, la commande `python` n’est pas toujours créée par défaut. Le système privilégie explicitement `python3`.

### Correction

J’ai installé `python-is-python3`, ce qui crée un lien symbolique entre `python` et `python3`. Ensuite, `python --version` a bien retourné la version de Python 3.

## Panne 5 — DNS cassé

### Symptôme

La commande `ping google.com` échouait avec une erreur de résolution de nom, alors que le réseau semblait toujours actif.

### Diagnostic

J’ai testé `ping 8.8.8.8`, qui fonctionnait correctement. Cela montrait que la connectivité IP était bonne. En consultant `/etc/resolv.conf`, j’ai vu qu’un serveur DNS incorrect avait été configuré.

### Cause racine

Le fichier de résolution DNS contenait un nameserver invalide, ce qui empêchait la traduction des noms de domaine en adresses IP.

### Correction

J’ai remis un nameserver valide, `127.0.0.53`, pour repointer vers le résolveur local géré par `systemd-resolved`. Après cela, la résolution DNS a refonctionné et `ping google.com` a réussi.

## Leçons apprises

Cette mission m’a appris qu’il ne faut pas corriger un problème au hasard. Il faut d’abord lire le message d’erreur exact, identifier la bonne couche technique, puis tester une hypothèse avant d’appliquer une correction.
J’ai aussi compris que beaucoup de pannes simples en Linux reviennent à quelques catégories : permissions, services, paquets manquants, commandes absentes et configuration réseau.
Enfin, j’ai retenu qu’une bonne habitude d’administrateur est de toujours garder un accès de secours avant de toucher à des services critiques comme SSH.
