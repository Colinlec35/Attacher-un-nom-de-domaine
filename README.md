Petit tuto pour rattacher de manière efficace son nom de domaine acheté chez OVH avec son site hébergé chez Heroku

Tout n'est pas instannée et peut prendre quelques dizaines de minutes, donc soyez patient entre chaque étape, et partez du principe que si rien ne s'est passé dans les 2h après votre requête c'est qu'il y a un problème, sinon Keep Cool 😉

## Prérequis

1) avoir acheté un nom de domaine chez OVH (pour ce tuto on admettra que vous possedez example.com)
2) avoir une app hébergée chez Heroku
3) avoir un forfait hobby sur heroku (pour pouvoir avoir son site en https)

## Travail sur Heroku

Posisitionnez vous dans le bon dossier et depuis votre terminal tapez la commande suivante :
```
heroku domains:add www.example.com
```
le 'www.' doit apparaitre avant votre nom de domaine.

Heroku va vous renvoyez une configuration de ce genre là 'abc.herokudns.com.'

## Travail sur OVH

Sur votre espace client ovh allez dans Domaines > example.com puis allez dans zone DNS.

Commencez par faire un peu de ménage dans la Zone DNS en supprimant tous les Type 'TXT'.

Puis ajoutez une entrée.

Selectionnez CNAME.

Remplissez les cases de la manière suivante:

Sous-domaine : www.example.com

TTL : Par défaut

cible: 'abc.herokudns.com.'

Attention vous devez mettre un '.' à la fin de votre cible après le .com, c'est primordial!

Validez votre config

## Travail sur heroku
Allez dans le settings de votre app et si vous avez un compte hobby générez un SSL pour que votre site apparaisse comme sécurisé.

Attendez quelques minutes et c'est bon vous pourrez enfin profitez de votre site www.example.com 

## Obliger les browser à renvoyer en https
Pour que http://www.example.com vous renvoie directement https://www.example.com il suffit simple d'ajouter dans config/environments/production.rb
`config.force_ssl = true` 

Attention à ce qu'il n'y ai pas d'image sur votre home page qui proviennent de site exterieur en http, sinon cela risque de vous renvoyer des erreurs.


## Obliger le Browser à afficher directement la page example.com en https
Afin que vous ne soyez pas obliger de rentrer systématiquement le www. avant votre root domain vous pouvez faire la manip suivante dans OVH.

1) Dans domaines/example.com/informations générales

Cliquer sur hebergement web et e-mail gratuit > Activer (inclus dans l'achat de votre domaine)

2) Dans domaines/example.com/zone DNS

supprimer l'entrée A sur www.example.com que la commande a dû ajouter (ne supprimez surtout pas l'entrée A sur example.com avec l'adresse IP).

3) Dans hébergements/example.com/multisite

Activez le SSL que sur le domaine root example.com (pas sur le www.example.com ni sur le serveur ovh)

4)  hébergements/example.com/informations générales 

Au besoin relancez le certificat SSL (ou activez le)

5) hébergements/example.com/FTP - SSH / FTP explorer

Ajouter le nouveau fichier suivant (et enregistrez le)

nom du fichier : `/www/.htaccess`

code: 

``` 
RewriteEngine On

	## no-www -> www
RewriteCond %{HTTP_HOST} !^www\.
RewriteRule ^ https://www.%{HTTP_HOST}%{REQUEST_URI} [R=301,L,NE,QSA]
```

6) Télécharger Filezilla

Ici pour le lien https://filezilla-project.org/

rentrez vos coordonées de serveur transmis par mail par ovh dedans et poussez le fichier `/www/.htaccess` que vous venez d'écrire


Voilà normalement tout marche vous pouvez taper  http://example.com ou http://www.example.com ou encore https://example.com et vous arriverez systématiquement sur votre page https://www.example.com

Bon courage!
