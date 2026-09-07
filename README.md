# TP1 : Connexion d'un routeur Cisco a un modem DSL avec les configurations FAI

Lab realise sous Cisco Packet Tracer dans le cadre du BUT Reseaux et Telecommunications (IUT de Saint-Pierre).

Le but est de relier un routeur client (SOHO_ROUTER) au routeur d'un fournisseur d'acces a Internet (ISP_Router) a travers un modem DSL simule par un nuage. Le cote FAI heberge un serveur DNS et un serveur Web. Le cote client dessert plusieurs VLAN grace a un routage inter-VLAN (router-on-a-stick). L'objectif final est qu'un poste du reseau client atteigne le serveur Web par son nom de domaine.

## Notions mises en pratique

- Ajout du module de commutation NM-ESW-161 sur un routeur
- Adressage des VLAN par interfaces virtuelles (SVI)
- Routage inter-VLAN avec la technique du router-on-a-stick et l'encapsulation 802.1Q
- Mise en place d'un serveur DHCP sur un routeur
- Configuration des services DNS et Web
- Liaison DSL via un Cloud
- Demarche de depannage methodique (verification des liens trunk)

## Topologie

Deux zones reliees par le modem DSL :

- Zone FAI : ISP_Router, serveur DNS, serveur Web
- Zone client : SOHO_ROUTER, un commutateur, le poste HR

## Plan d'adressage

| Equipement   | Interface | Adresse IP / Masque      | Role                                  |
|--------------|-----------|--------------------------|---------------------------------------|
| ISP_Router   | VLAN 8    | 8.8.8.1 / 24             | Passerelle du reseau du serveur DNS   |
| ISP_Router   | VLAN 10   | 10.10.10.1 / 24          | Passerelle du reseau du serveur Web   |
| ISP_Router   | Fa0/0     | 20.110.24.1 / 24         | Liaison vers le modem DSL (cote FAI)  |
| Serveur DNS  | -         | 8.8.8.8 / 24             | Resolution des noms de domaine        |
| Serveur Web  | -         | 10.10.10.10 / 24         | Hebergement du site www.google.com    |
| SOHO_ROUTER  | Fa0/0     | 20.110.24.2 / 24 (DHCP)  | Liaison WAN vers le FAI               |
| SOHO_ROUTER  | Fa0/1.5   | 172.16.5.1 / 24          | Passerelle du VLAN 5                  |
| SOHO_ROUTER  | Fa0/1.10  | 172.16.10.1 / 24         | Passerelle du VLAN 10                 |
| PC HR        | Fa0       | 172.16.5.10 / 24         | Poste client dans le VLAN 5           |

## Prerequis

- Cisco Packet Tracer version 9.0.0

## Comment ouvrir le lab

1. Cloner le depot ou telecharger les fichiers.
2. Ouvrir le fichier `TP1_ISP_DSL.pkt` avec Packet Tracer.
3. La maquette est déjà configurée et fonctionnelle.

## Tests a realiser

Depuis le PC HR (onglet Desktop) :

1. Ping vers la passerelle du VLAN 5 :
   ```
   ping 172.16.5.1
   ```
2. Ping vers l'interface WAN du routeur client :
   ```
   ping 20.110.24.2
   ```
3. Ping vers l'interface WAN du FAI :
   ```
   ping 20.110.24.1
   ```
4. Acces au serveur Web dans le navigateur :
   ```
   http://www.google.com
   ```

Le dernier test valide toute la chaine : routage de bout en bout, distribution DHCP, resolution DNS et reponse du service HTTP.

## Contenu du depot

- `README.md` : ce fichier
- `TP1_ISP_DSL.pkt` : la maquette Packet Tracer
- `CR_TP1.docx` : le compte rendu detaille du TP

## Auteurs

Groupe : DILMAHAMOD Réhaan, Bouchrani Ambdouroihamane
