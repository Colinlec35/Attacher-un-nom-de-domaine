Petit tuto pour rattacher de manière efficace son nom de domaine acheté chez OVH avec son site hébergé chez Heroku

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

Sur votre espace client ovh allez dans Domaines > example.com puis allez dans zone DNS

Commencez par faire un peu de ménage dans la Zone DNS en supprimant tous les Type 'TXT'

Puis ajoutez une entrée
Selectionnez CNAME

Remplissez les cases de la manière suivante:
Sous-domaine : www.example.com
TTL : Par défaut
cible: 'abc.herokudns.com.'

Attention vous dezvez mettre un '.' à la fin de votre cible après le .com, c'est primordial!

validez votre config

## Travail sur heroku
allez dans le settings de votre app et si vous avez un compte hobby générez un SSL pour que votre site apparaisse comme sécurisé

Attendez quelques minutes et c'est bon vous pourrez enfin profitez de votre site www.example.com 