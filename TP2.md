# 🌐 Rapport de TP : Architecture et Protocoles Réseau

Ce dépôt documente les manipulations réseaux effectuées pour comprendre le fonctionnement de la pile TCP/IP, la gestion des adresses et les services essentiels (DHCP/DNS).

---

## I. Exploration de la Configuration Locale

L'étape initiale consiste à identifier comment la machine est vue par le réseau.

### 1. Analyse des interfaces (WiFi & Ethernet)
À l'aide des menus système et de la ligne de commande, nous avons extrait les identifiants uniques.

* **L'adresse MAC** : Identifiant physique de la carte.
* **L'adresse IP** : Identifiant logique sur le réseau local.

![Capture des paramètres WiFi](https://raw.githubusercontent.com/Fireweather/linux/main/image/parametreWifi.png)
> *Ici, on accède à la liste des réseaux disponibles et aux propriétés de connexion pour isoler l'interface active.*

![Détails techniques de la carte](https://raw.githubusercontent.com/Fireweather/linux/main/image/detailWifi.png)
> *Cette vue détaillée montre le masque de sous-réseau (ex: 255.255.255.0) qui nous permet de calculer l'adresse de diffusion (broadcast) et l'adresse réseau.*

---

## II. Gestion de l'Adressage et Sécurité

### A. Modification manuelle (Statique vs Dynamique)
Pour comprendre l'importance du DHCP, nous avons configuré manuellement une adresse IP.

![Configuration IP manuelle](https://raw.githubusercontent.com/Fireweather/linux/main/image/changementIpGraphique.png)
> *Saisie manuelle des paramètres : IP, Masque et Passerelle (Gateway). Sans une Gateway correcte, la machine peut communiquer en local mais ne peut plus sortir sur Internet.*

### B. Éviter les conflits avec Nmap
Avant de choisir une IP statique, il est crucial de vérifier qu'elle n'est pas déjà utilisée par un autre hôte pour éviter un "conflit d'IP".

![Scan Nmap pour IP libre](https://raw.githubusercontent.com/Fireweather/linux/main/image/ipNonOccuper.png)
> *Utilisation de `nmap -sn` : La commande scanne la plage réseau. Si une adresse ne répond pas, elle est considérée comme libre pour notre configuration.*

![Validation du changement](https://raw.githubusercontent.com/Fireweather/linux/main/image/ipChange.png)
> *Vérification après modification : On constate que la nouvelle interface possède bien l'adresse IP choisie suite au scan.*

---

## III. Services Réseau : DHCP et DNS

### 1. Le mécanisme DHCP
Le serveur DHCP "loue" une adresse IP à l'ordinateur pour une durée déterminée.

* **Le Bail (Lease) :** Les images ci-dessous montrent les détails du contrat entre le client et le serveur.

![Serveur DHCP et Bail](https://raw.githubusercontent.com/Fireweather/linux/main/image/information%20complette%20serveur%20dhcp%20(bail).png)
> *Sur cette capture, on voit précisément la date d'obtention de l'IP et sa date d'expiration. C'est le serveur DHCP d'Ingésup qui gère cette distribution.*

![Renouvellement forcé](https://raw.githubusercontent.com/Fireweather/linux/main/image/chagement%20de%20bail%20(et%20possiblement%20d'ip).png)
> *Action de libérer l'adresse (`release`) et de demander un nouveau bail (`renew`) via le terminal pour forcer une mise à jour de la configuration.*

### 2. La Résolution DNS
Le DNS traduit les noms (google.com) en adresses IP.

![Interrogations DNS](https://raw.githubusercontent.com/Fireweather/linux/main/image/serveur%20dns.png)
> *Utilisation de `nslookup` : On interroge le serveur DNS pour obtenir l'IP de domaines spécifiques. On voit ici la réponse "Non-authoritative answer" qui indique que l'info vient d'un cache.*

![Tests DNS complémentaires](https://raw.githubusercontent.com/Fireweather/linux/main/image/nsclimp.png)
> *Vérification de la connectivité du résolveur DNS local et tests de résolution inverses.*

---

## 🛠 Outils utilisés
* **Analyse** : `ipconfig /all`
* **Scan** : `nmap` (Network Mapper)
* **DNS** : `nslookup`
* **Flux** : `release / renew` pour le DHCP

---