# SAE Réseau

## Calcul des sous-réseaux

L'adresse à notre disposition est 170.50.192.0/24. Nous allons diviser cette adresse en trois sous-réseaux, un pour chaque bâtiment. Si on divise le réseau en trois sous-réseaux de manière égale, on obtient trois réseaux de 64 adresses chacun. Or, le bâtiment Production doit accueillir 110 postes de travail.

De ce fait, on va diviser le réseau en deux sous-réseaux, un pour le bâtiment Production et un qui sera partagé entre les deux autres bâtiments. 

Pour le bâtiment Production, on peut utiliser un masque de sous-réseau de 25 bits, ce qui donne un masque de sous-réseau de 170.50.192.0/25. Cela nous donne 128 adresses, dont 126 utilisables. Ainsi, ce sous-réseau peut accueillir les 110 postes de travail.

Pour les deux autres bâtiments, on vas diviser en deux le sous-réseau restant, soit le sous-réseau suivant : 170.50.192.128/25. On peut utiliser un masque de sous-réseau de 26 bits, ce qui donne un masque de sous-réseau de 170.50.192.128/26 pour le batiment Finances et un masque de sous-réseau de 170.50.192.192/26 pour le bâtiment R&D. Cela nous donne 64 adresses, dont 62 utilisables, pour chaque sous-réseau. 

Néanmoins, on a besoin de 3 adresses pour les routeurs. On va donc utiliser un masque de sous-réseau de 27 bits, ce qui donne un masque de sous-réseau de 170.50.192.192/27 pour le bâtiment R&D. 

Pour les liaisons entre les routeurs, on va donc diviser le sous-réseau restant en trois, soit le sous-réseau suivant : 170.50.192.224/27. Ainsi, les adresses des liaisons entre les routeurs seront les suivantes :

R1-R2 : 170.50.192.224/29
R1-R3 : 170.50.192.232/29
R2-R3 : 170.50.192.240/29






Les adresses des postes de travail dans tous les bâtiments seront attribuées dynamiquement. Un seul serveur DHCP sera utilisé, avec des relais DHCP dans chaque sous-réseau. De plus, les dix premières adresses de chaque sous-réseau seront réservées pour l’installation de serveurs avec des adresses statiques.


## Annexe

### Configuration des routeurs

#### Commande de R1

configure terminal

interface e0/0
ip address 170.50.192.126 255.255.255.128
no shutdown

interface e0/1
ip address 170.50.192.229 255.255.255.248
no shutdown

interface e0/2
ip address 170.50.192.237 255.255.255.248
no shutdown
end
wr

#### Commande de R2

configure terminal

interface e0/0
ip address 170.50.192.190 255.255.255.192
no shutdown

interface e0/1
ip address 170.50.192.230 255.255.255.248
no shutdown

interface e0/2
ip address 170.50.192.245 255.255.255.248
no shudown
end
wr

#### Commande de R3

configure terminal

interface e0/0
ip address 170.50.192.238 255.255.255.248
no shutdown

interface e0/1
ip address 170.50.192.246 255.255.255.248
no shutdown

interface e0/2
ip address 170.50.192.222 255.255.255.224
no shutdown
end
wr


### Configuration des PC

#### Commande de PC1

ip 170.50.192.1 170.50.192.126
save

#### Commande de PC2

ip 170.50.192.129 170.50.192.190
save

#### Commande de PC3
 
ip 170.50.192.193 170.50.192.222
save