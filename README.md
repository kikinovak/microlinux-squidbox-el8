# Microlinux Squidbox EL8

Configuration Ansible pour mon *routerboard* PC Engines APU3
`squidbox.microlinux.lan` sous Rocky Linux 8.


## Installation

- Brancher l'adaptateur USB sur le port série du *routerboard*.

- Relier le port `WAN` au réseau local.

- Ne pas brancher le port `LAN` pour l'instant.

- Lancer `minicom` sur le PC externe.

- Brancher la clé USB d'installation.

- Allumer le *routerboard*.

- Appuyer successivement sur **F10** et sur **1** pour démarrer sur la clé.

- Appuyer sur **FlècheHaut** puis sur **Tab** pour sélectionner les options de
  `Install Rocky Linux 8.x`.

- Ajouter `inst.vnc inst.vncpassword=rocky8 edd=off` aux options du kernel et
  démarrer.

- Surveiller le démarrage du *routerboard* dans le réseau local avec `watch
  nmap squidbox` et guetter l'ouverture du port (`5901/tcp open  vnc-1`) pour
  la connexion VNC.

- Lancer KRDC sur le PC externe et initier une connexion à l'adresse
  `192.168.2.251:1`.

- Fournir le mot de passe `rocky8` pour la connexion VNC.

- L'installateur Anaconda s'affiche dans KRDC.

- *Sélection Logiciel* : **Installation Minimale**

- Désactiver Kdump.

- Définir un mot de passe pour `root`.

- Ne pas créer de compte utilisateur, Ansible s'en chargera.

- Redémarrer au bout de l'installation sans oublier d'enlever la clé USB
  d'installation.

> Dans cette configuration le *routerboard* reçoit déjà une adresse IP fixe
> `192.168.2.251` dans le réseau local en fonction de l'adresse MAC de
> l'interface `enp1s0` étiquetée `WAN`.


## Configuration post-installation

Mettre en place l'authentification par clé SSH :

```
$ ssh-copy-id -i ~/.ssh/id_rsa.pub root@squidbox
```

Se connecter au *routerboard* :

```
$ ssh root@squidbox
```

Effectuer la mise à jour initiale :

```
# dnf update -y
```

Redémarrer :

```
# reboot
```

Activer le dépôt de paquets EPEL :

```
# dnf install -y epel-release
```

Installer Ansible :

```
# dnf install -y ansible
```

Préparer le terrain pour Ansible :

```
# ansible-pull -U https://gitlab.com/kikinovak/microlinux-squidbox-el8 \
  bootstrap.yml
```

Lancer la configuration du *routerboard* :

```
# ansible-pull -U https://gitlab.com/kikinovak/microlinux-squidbox-el8
```

Redémarrer :

```
# reboot
```

