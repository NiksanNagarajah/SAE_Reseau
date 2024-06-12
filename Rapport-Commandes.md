Nagarajah Nkisan

Wiciak Alexy

1-1A

# SAE Réseau

- lien de notre github : https://github.com/Snakin32/SAE_Reseau

## Calcul des sous-réseaux

L'adresse à notre disposition est 170.50.192.0/24. Nous allons diviser cette adresse en trois sous-réseaux, un pour chaque bâtiment. Si on divise le réseau en trois sous-réseaux de manière égale, on obtient trois réseaux de 64 adresses chacun. Or, le bâtiment Production doit accueillir 110 postes de travail.

De ce fait, on va diviser le réseau en deux sous-réseaux, un pour le bâtiment Production et un qui sera partagé entre les deux autres bâtiments. 

### Batiment Production :

Pour le bâtiment Production, on peut utiliser un masque de sous-réseau de 25 bits, ce qui donne un masque de sous-réseau de 170.50.192.0/25. Cela nous donne 128 adresses, dont 126 utilisables. Ainsi, ce sous-réseau peut accueillir les 110 postes de travail.

### Batiment Finances et R&D :

Pour les deux autres bâtiments, on vas diviser en deux le sous-réseau restant, soit le sous-réseau suivant : 170.50.192.128/25. On peut utiliser un masque de sous-réseau de 26 bits, ce qui donne un masque de sous-réseau de 170.50.192.128/26 pour le batiment Finances et un masque de sous-réseau de 170.50.192.192/26 pour le bâtiment R&D. Cela nous donne 64 adresses, dont 62 utilisables, pour chaque sous-réseau. 

Néanmoins, on a besoin de 3 adresses pour les routeurs. On va donc utiliser un masque de sous-réseau de 27 bits, ce qui donne un masque de sous-réseau de 170.50.192.192/27 pour le bâtiment R&D. 

Pour les liaisons entre les routeurs, on va donc diviser le sous-réseau restant en trois, soit le sous-réseau suivant : 170.50.192.224/27. Ainsi, les adresses des liaisons entre les routeurs seront les suivantes :

- R1-R2 : 170.50.192.224/29

- R1-R3 : 170.50.192.232/29

- R2-R3 : 170.50.192.240/29

## Annexe

### Configuration des routeurs

#### Commandes de R1

```
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

conf term
ip route 170.50.192.128 255.255.255.192 170.50.192.230
ip route 170.50.192.192 255.255.255.224 170.50.192.238
end
wr
```

###### Configuration du serveur DHCP

```
conf term
service dhcp
ip dhcp pool production
network 170.50.192.0 255.255.255.128
lease 1
default-router 170.50.192.126
end
wr

conf term
ip dhcp excluded-address 170.50.192.1 170.50.192.10
end
wr

conf term
service dhcp
ip dhcp pool finances
network 170.50.192.128 255.255.255.192
lease 1
default-router 170.50.192.229
end
wr

conf term
ip dhcp excluded-address 170.50.192.129 170.50.192.139
end
wr

conf term
service dhcp
ip dhcp pool r&d
network 170.50.192.192 255.255.255.224
lease 1
default-router 170.50.192.237
end
wr

conf term
ip dhcp excluded-address 170.50.192.193 170.50.192.203
end
wr
```

###### Configuration du routage dynamique DHCP

```
configure terminal
router rip
version 2
no auto-summary
network 170.50.192.0
network 170.50.192.224
network 170.50.192.232
end
wr
```

#### Commandes de R2

```
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

conf term
ip route 170.50.192.0 255.255.255.128 170.50.192.229
ip route 170.50.192.192 255.255.255.224 170.50.192.246
end
wr
```

###### Configuration du relai DHCP

```
configure terminal
interface e0/0
ip helper-address 170.50.192.229
end
wr
```

###### Configuration du routage dynamique DHCP

```
configure terminal
router rip
version 2
no auto-summary
network 170.50.192.128
network 170.50.192.224
network 170.50.192.240
end
wr
```

#### Commandes de R3

```
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

conf term
ip route 170.50.192.0 255.255.255.128 170.50.192.237
ip route 170.50.192.128 255.255.255.192 170.50.192.245
end
wr
```

###### Configuration du relai DHCP

```
configure terminal
interface e0/2
ip helper-address 170.50.192.237
end
wr
```

###### Configuration du routage dynamique DHCP

```
configure terminal
router rip
version 2
no auto-summary
network 170.50.192.192
network 170.50.192.240
network 170.50.192.232
end
wr
```

#### Commandes de R4

```
configure terminal
interface e0/0
ip address 10.0.0.1 255.255.255.0
no shutdown
end
wr

configure terminal
interface e0/1
ip address 170.50.192.125 255.255.255.128
no shutdown
end
wr

configure terminal
router rip
version 2
no auto-summary
network 10.0.0.0
network 170.50.192.0
end
wr
```
###### Configuration du Pare-feu

```
configure terminal
ip access-list standard monACL
deny 170.50.192.0 0.0.0.255
permit any
end
wr

configure terminal
interface e0/1
ip access-group monACL in
end
wr
```

### Configuration des PC

#### Commande de PC1

```
ip 170.50.192.1/25 170.50.192.126
save
```

#### Commande de PC2

```
ip 170.50.192.129/26 170.50.192.190
save
```

#### Commande de PC3

```
ip 170.50.192.193/27 170.50.192.222
save
```
#### Commande de Internet

```
ip 10.0.0.2/24 10.0.0.1
save
```

