# vigilant-guide

## Scanner de log 

Ce script python permet de scanner des logs (apache ou auth acess log), 
une fois analysé les erreurs présentes dans les logs (401, 403, 404 , ainsi que les possible break-in attempt)
peuvent être affichées sur la console, 
ou bien un rapport peut être crée automatiquement et envoyé par mail (avec un résumé des erreurs inscrit dans le mail). 
Ce script peut être interactif, ce qui permet de choisir les options désirées, 
ou bien s'effectuer automatiquement pour qu'un mail soit envoyé et un rapport crée, 
ceci pour gagner du temps. Le script est capable de lire des logs même si il sont compressé au fomat gzip.

## Installation : 
Il faut créer un dossier logs contenant les logs à analyser, 
Télécharger le script et le mettre dans le même dossier que le dossier logs. 
Il faut également mettre votre adresse mail pour envoyer l'analyse des log 
(l'adresse mail peut être la même, ou vous pouvez utiliser une adresse mail pour l'envoi et une pour la réception) ; 
il faut également mettre le smtp de la messagerie utilisé par votre adresse mail d'envoi 
(exemple pour le smtp de google il faut utiliser : smtp.google.com ; 
mais aussi autoriser l'accès pour les applications moins sécurisées pour permettre l'utilisation de l'envoi du mail par le script ;
cette option est disponible dans les paramètres de votre adresse mail).

## Usage : 

L'utilisation peut se faire soit automatiquement (si on lance le script sans argument) ; 
il créera alors un rapport d'erreurs dans le document où est placé le dossier logs et le script, 
et enverra un email avec le rapport ainsi que le nombres des erreurs trouvées. 
Ou bien en utilisant l'argument all il deviendra interactif et proposera d'afficher le rapport d'erreur sur la console, 
ou/et de créer le rapport dans un dossier errors crée automatiquement et de l'envoyer par mail.

## Usage avancé : 

Il est possible également de rajouter des erreurs que vous souhaitez rechercher dans les logs, 
et même de rajouter d'autres type de logs à analyser. 
Pour cela il suffit d'ajouter un dictionnaire dans la fonction 2 pour chaque types de logs que vous voulez analyser 
ou/et de rajouter une liste pour chaque erreurs que vous voulez rechercher.

## Licence

Ce script est sous licence Apache, Version 2.0
