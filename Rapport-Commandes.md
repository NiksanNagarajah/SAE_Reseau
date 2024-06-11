# SAE Réseau

## Calcul des sous-réseaux

L'adresse à notre disposition est 170.50.192.0/24. Nous allons diviser cette adresse en trois sous-réseaux, un pour chaque bâtiment. Si on divise le réseau en trois sous-réseaux de manière égale, on obtient trois réseaux de 64 adresses chacun. Or, le bâtiment Production doit accueillir 110 postes de travail.

De ce fait, on va diviser le réseau en deux sous-réseaux, un pour le bâtiment Production et un qui sera partagé entre les deux autres bâtiments. 

Pour le bâtiment Production, on peut utiliser un masque de sous-réseau de 25 bits, ce qui donne un masque de sous-réseau de 170.50.192.0/25. Cela nous donne 128 adresses, dont 126 utilisables. Ainsi, ce sous-réseau peut accueillir les 110 postes de travail.

Pour les deux autres bâtiments, on vas diviser en deux le sous-réseau restant, soit le sous-réseau suivant : 170.50.192.128/25. On peut utiliser un masque de sous-réseau de 26 bits, ce qui donne un masque de sous-réseau de 170.50.192.128/26 pour le batiment Finances et un masque de sous-réseau de 170.50.192.128/26 pour le bâtiment R&D. Cela nous donne 64 adresses, dont 62 utilisables, pour chaque sous-réseau. 









Les adresses des postes de travail dans tous les bâtiments seront attribuées dynamiquement. Un seul serveur DHCP sera utilisé, avec des relais DHCP dans chaque sous-réseau. De plus, les dix premières adresses de chaque sous-réseau seront réservées pour l’installation de serveurs avec des adresses statiques.


## Annexe

### Configuration des routeurs

#### Commande de R1



### Configuration des PC

#### Commande de PC1

ip 170.50.192.5

... 