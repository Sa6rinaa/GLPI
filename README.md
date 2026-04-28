<div style="text-align:left;">Protière Sabrina  | BTS SIO1</div>


---
## <center> Compte Rendu GLPI</center>

Le but de ce compte rendu est de créer un parc informatique avec un serveur GLPI, pour réaliser cela il faut d'abord ce rendre sur [https://vcenter.swsio.local](https://vcenter.swsio.local) et créer trois machine virtuelle, l'une sur windows 10 et deux Debian avec/sans interface graphique. Pour la création de ce parc informatique il faut installer un LAMP. Le LAMP va permettre de faire fonctionner GLPI. Ensuite il va falloir installer GLPI et dessus nous pourrons gérer le parc informatique de GLPI.


### Installation Debian



Pour lancer l'installation d'une Debian avec et sans interface graphique (avec les machines virtuelles préalablement créées dans [https://vcenter.swsio.local](https://vcenter.swsio.local)):


1. Choisir sa langue (France).
2. Nommez votre machine avec le nom que vous souhaitez.
3. Choisir un mot de passe root -> Choisir un nom d'utilisateur -> Choisir un login pour l'utilisateur -> Choisir un mot de passe.(Pour la création d'un compte sur la Debian)
4. Demande de la Partition : disque entier-> sda -> une partition -> appliquer les changements : oui (de la place que va prendre sur votre machine)

![](image/1.png) 
![](image/2.png)


![](image/3.png)
![](image/4.png)

5. Choisir son pays : France

6. Mettre deb.debian.org pour l'archive comme ci-dessous :

 ![](image/5.png)

7. Ne rien mettre pour HTTP -> Etude stat : non -> Sélection des logiciels : cocher uniquement les deux premiers comme ci-dessous :

 ![](image/6.png)

**8. Cocher Oui pour installer le programme de démarrage GRUB** [^1]

![](image/7.png)

9. Dans les choix périphérique, choisir : /dev/sda

![](image/8.png)

***Répéter les instructions ci-dessus pour les deux autres machines virtuelles (Debian avec interface graphique et pour la Windows 10).***


 ---

### Installation d’un serveur LAMP

Un serveur LAMP permet de construire des serveurs de sites web, acronyme de Linux (le système d'exploitation), Apache (le serveur Web), MariaDB (anciennement MySQL), (le serveur de base de données) et PHP (le langage de script). Donc pour la création de ce LAMP, il faut commencer par installer MariaDB, PHP et Apache :


1. Installation de Mariadb/Apache :
  - Lancer le terminal de votre machine.
  - Se mettre en administrateur grâce à la commande : `su` 
  - Mettre à jour sa machine avec les deux commandes : `apt update` `apt upgrade`
  - Installation d'Apache ou Mariadb : `apt install apache2` `apt install mariadb`



2. Installation de PHP :

- `apt install lsb-release apt-transport-https ca-certificates software-propperties-common -y` : On installe  
- `wget -0 /etc/apt/trusted.gpg.d/php.gpg https://packages.sury.org/php/apt.gpg`: Téléchargement d'un ficher sur internet
- `sh -c ‘echo ”deb https://packages.sury.org/php/ $(lsb_release -sc)main”> /etc/apt/sources.list.d/php.list'`
- `apt update` : On met à jour
- `apt install php8.2` : Installation de php8.2


3. Vérification de l'installation :
- Pour vérifier que votre installation d'Apache a bien été réalisée effectuer la commande : `systemctl status apache2` (si cela à bien été réalisée vous devez apercevoir dans "Active":"active running")


Vous devez obtenir ce résultat  :  
![](image/9.png) 


*Synthèse de cette 1er partie : Nous avons installé un serveur LAMP sur l'une des machines Debian . Nous avons PHP, Mariadb et Apache. Nous avons donc la plateforme pour faire fonctionner GLPI. Or nous n'avons pas encore GLPI il faut donc l'installer pour que l'on puisse obtenir un parc informatique.*

---
### Installation de GLPI 10.0.6

GLPI (Gestionnaire Libre de Parc Informatique) est un logiciel libre de gestion des services informatiques et de gestion des services d’assistance. Il permet de gérer les machines que l'on a dans un parc informatique.

Pour effectuer l'installation de GLPI, il faut d'abord créer la base de données GLPI ;

1. Dans un premier temps, il faut ce connecter à Mariadb pour la crétion dune base de données qui va nous servir plus tard pour l'installtion de GLPI avec les commandes ci-dessous :

- Entrer le mot de passe que vous avez defini plus tôt avec :`mysql -u root -p`
- Création de la base de données "glpi" :`create database glpi;`
- Choisir le nom d'utilisateur avec le choix du mot de passe
- `create user’glpi’@’localhost’identified by‘glpi’;`
- On augmente les droits de l'utilisateur avec la commande : `grant all privileges on glpi.*to’glpi’@’localhost’ with grant option;`
- Mettre jour à les modifications effectuées : `flush privileges;`
- Pour finir, quitter Mariadb avec la commande : `exit`




2. Dans un deuxième temps nous allons installer GLPI notamment grâce aux liens ci-dessous :
- Téléchargement du logiciel gpli avec sa version la plus récente (10.0.10) :`wget https://github.com/glpi-project/glpi/releases/download/10.0.10/glpi-10.0.10.tgz`
- Le décompresser en saisissant la commande : `tar xvf glpi-10.0.10.tg`
- Déplacer ce dossier décompressé nommé « glpi » dans l’arborescence d’Apache commande à réaliser: `mv glpi /var/www/html/glpi`
- Installation des diffférents modules : `apt install  php8.2-curl php8.2-gd php8.2-mbstring php8.2-zip php8.2-xml php8.2-ldap php8.2-intl php8.2-mysql php8.2-dom php8.2-simplexml php-json php8.2-phpdbg php8.2-cgi`
- Ensuite, il faut donner la propriété du dossier GLPI à l’administrateur d’Apache grâce à ces deux commandes :`chown -R www-data:www-data /var/www/html/glpi/` et  ` chmod -R 755 /var/www/html/glpi/`
- On redémarre le server Apache avec :`systemctl restart apache2 `


 Avec l'adresse IP récupérée précédemment (avec la commande `ip a`). Lancer dans la barre de recherche de votre navigateur votre IP..**/glpi** (exemple : 172.26.122.3/glpi)

On obtient :


![](image/10.png)

 - On obtient la page d'installation de GLPI, pour cela il faut suivre les étapes demandée. Mettez-le en français, acceptez le contrat d'utilisation et débutez l'installation comme ci-dessous : 

 ![](image/11.png) ![](image/12.png) ![](image/13.png) 

 - Ensuite il faut se connecter au serveur Sql (Mariadb) que l'on a créer précédemment et on sélectionne le base GLPI ensuite, attendre l'initialisation de la base :
 
![](image/14.png) ![](image/15.png) ![](image/16.png) 


- On se connecte en administrateur :

![](image/17.png)

- Par mesure de sécurité, on supprime le fichier "install.php" avec la commande :`rm -f /var/www/html/glpi/install/install.php`


Le résultat que vous devez avoir globalement (dans la central vous ne devez pas avoir 124 logiciel, 1 ordinateur et c'est absolument normal ):

![](image/38.png)


*Synthèse de cette 2ème partie : Nous avons installé un serveur LAMP sur l'une des machines Debian et nous avons installer GLPI (image ci-dessus). Nous avons donc le parc informatique de nos machines, mais nos machines ne sont pas encore remonter sur GLPI. Il faut donc faire remonter les deux Debian et Windows pour que le parc informatique fonctionne.Pou cela il faut installer Fusioninventory sur les deux machines Debian et la Windows 10.*

---

### Installation de Windows 10

Après le téléchargement de la Windows sur  [https://vcenter.swsio.local](https://vcenter.swsio.local) ,il faut suivre le protocole que microsoft demande comme ci-dessous :


![](image/34.png)   ![](image/35.png)   ![](image/36.png)   ![](image/37.png) 

><div style="text-align:left;"> Sélectionner sa langue/disposition de clavier  -> Création de votre compte (choix du mot de passe et nom d'utilisateur) -> refuser la localisation à Microsoft et la localisation de votre appareeil -> Refuser l'expérience personnalisée -> Refuser l'expérience personnaliser/utilisateur de publicité</div>


---
### Installation de Plugin FusionInventory
FusionInventory est un plugin (s’intégrant dans GLPI) et un agent d’inventoring, permettant d'automatiser la remontée d'informations depuis les postes à inventorier de votre parc, vers votre serveur GLPI.Il permet de faire la maitenance sur le parc informatique grâce a GLPI.


1. Installation du paquet "unzip" avec : `apt install unzip`
2. Installation de fusion inventory dans sa version la plus récente (10.0.6):`wget https://github.com/fusioninventory/fusioninventory-for-glpi/releases/download/glpi10.0.6%2B1.1/fusioninventory-10.0.6+1.1.zip`
3. Décompression du plugin : `unzip fusioninventory-10.0.3+1.0.zip` 
3. La création d'un dossier a été effectuée, on déplace ce dossier dans l’emplacement d’installation de GLPI grâce à : `mv fusioninventory /var/www/html/glpi/plugins/`
4. Pour vérifier l'emplacement du plugin Fusion inventory :`ls` . On devrait obtenir comme ci-dessous :

![](image/18.png)


### INSTALLATION DU PLUGIN FUSIONINVENTORY DANS GLPI 10


>Après la connexion sur le tableau de bord de votre GLPI , dirigez-vous dans le menu à gauche , cliquez sur "Configuration" et "Plugins",le plugin FusionInventory devrait apparaître. Ainsi, cliquez sur le "+" comme ci-dessous :

![](image/19.png)

>Cliquez sur le bouton situé à droite de l'écran de la colonne intitulée "Actions" (si le bouton activé il est censé être vert) :

![](image/20.png)

### Installation de FusionInventory sur chaque machine

Il faut maintenant installer FusionInventory sur chaque machine pour  être remontée dans l’inventaire GLPI (dans le parc informatique).

1. Sur une machine Windows 
- Connectez-vous sur le site officiel de FusionInventory : [http://fusioninventory.org/](http://fusioninventory.org/)
- Cliquez sur le bouton "Agent" et cliquez sur le lien situé dans la rubrique "Get the installer".
- Dans la page Github, cliquez sur le lien x64 comme ci-dessous: 




![](image/21.png)

> Double-cliquez sur le lien afin de lancer l'installation et suivez l'assistant d'installation comme ci-dessous: 

 ![](image/23.png) ![](image/24.png) ![](image/22.png) 


>Mettre en français, cliquez sur "suivant" et accepter les conditions d'utilisation.

 <div style="text-align:left;">       </div>



 <div style="text-align:left;"> .   </div>

![](image/25.png) ![](image/44.png)  ![](image/26.png) ![](image/27.png) ![](image/28.png)

> Cliquez, ici, le bouton « Suivant » (pas de paramétrage des options SSL) mettre comme liens : http://debian127.54.2.1/glpi/plugins/fusioninventory (c'est un lien non sécurisé avec le nom de votre ip) et encore « Suivant » (pas de proxy dans notre cas). Choisissez un mode d’exécution (par défaut le mode service est activé) et cliquez le bouton « Suivant ».


 ![](image/30.png) ![](image/31.png) ![](image/32.png) 

> Cliquez sur le bouton « Suivant » et cliquer sur la case « Lancer un inventaire immédiatement après l’installation» et lancer l'installation.

*Synthèse de cette 3ème partie : Nous avons installé un serveur LAMP sur l'une des machines Debian et nous avons installer GLPI (image ci-dessus), nos machines sont remonté a GLPI. Nous pouvons conclure que notre parc informatique est fonctionnelle.Maintenant nous pouvons lister des licences et suivre leurs utilisation pour rendre ce parc informatique plus "réelle".*

---

### Lister les licences et suivre leur utilisation

Dans GLPI on peux créer des licences avec différentes utilisation. Pour faire cela il faut aller dans GLPI sur l'onglet à gauche , "cliquez sur Gestion :


![](image/39.png) --> Choisir le contact/fournisseur/contrat que vous souhaitez créer,


![](image/40.png) --> Dans cette exemple je créer un contrat , pour le créer il faut cliquer un haut de votre page sur "+ Ajouter".



![](image/41.png) --> Ensuite créer votre contrat avec les informations que vous possédez et cliquer sur bouton "+ ajouter" en jaune en bas droite de votre écran.


![](image/42.png) 

>Retournez dans gestion/contrat vous devriez voir apparaître le contrat que vous venez de crée comme ci-dessus.

**Si vous souhaitez créer un fournisseur, un contrat ou autre la méthode est la même de precedemment.**


Maintenant que l'on a crée des contactes/contrats/fournisseurs il est possible de les liées ( liée un contrat à un fournisseur par exemple).

![](image/43.png) 

>Cliquez sur le contrat que vous voulez liée avec un fournisseur, ensuite cliquer dans action qui vous afficheras un page similaire à celle de gauche, il faut descendre le déroulant pour choisir le fournisseur, contacte que vous souhaitez ajouter à ce contrat.


*Conclusion: Nous avons un parc informatique fonctionnelle avec 3 machines virtuelle que l'on a remontée sur GLPI,la simulation est fonctionnelle. Il ne manque plus que la gestions de ce parc informatique pour l'optimiser.*




---

**[^1] Si vous n’avez pas sélectionné le /dev/sda pour GRUB :**

Lancer votre machine sur le CD d’installation et lancer la machine sur 
Advanced -> rescue mode -> sélectionné la langue -> Faites non pour la route par défaut -> sélectionné continuer jusqu’à  périphérique à monter comme système racine -> sélectionné /dev/sda1 -> exécuter un shell en ligne de commande -> effectuer ces 2 commandes : 
`grub-install /dev/sda `
`update-grub`
puis taper `exit` et redémarrer le système
Booter sur le disque dur




