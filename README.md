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

# Étape 1 : Téléchargez l'exploit dans votre ordinateur.

Pour cela, ouvrez un terminal en tant que root et téléchargez l'exploit à partir d'[ICI](https://raw.githubusercontent.com/rapid7/metasploit-framework/master/modules/exploits/unix/ftp/proftpd_modcopy_exec.rb)
