[Accueil](/index.md) | [Certifications](/certifications.md) | [Compétences](/competences.md) | [Projets](/projets.md) | [Scripts](/scripts.md)
# Ajout d'un hôte dans Zabbix Server

1. Se connecter à l'interface Web de Zabbix  
![image](https://pierremonot.github.io/portfolio/zabbixbasic-dash.png)

2. Sur la gauche, aller dans "Data Collection" -> "Hosts"  
![image](https://pierremonot.github.io/portfolio/zabbix-datacollection.png)

3. En haut à droite, cliquer sur "Create Host"  
![image](https://pierremonot.github.io/portfolio/zabbix-createhost.png)

4. Dans la page qui s'ouvre renseigner les différents paramètres
- Host name : le nom d'hôte de votre machine distante
- Visible name : (si vous voulez qu'elle apparaisse sous un autre nom dans votre interface)
- Template : Vous pouvez mettre le template qui s'approche le plus à votre machine/équipements réseau (ici nous utiliserons Linux by Zabbix agent mais vous pouvez utiliser aussi Windows by Zabbux agent)
- Host groups : Pour créer un groupe dédié (Production - Linux Server par exemple) à la machine ou l'ajouter à un existant
- Interfaces : Choisir Agent si c'est un agent ou SNMP si c'est un équipement réseau ou NAS par exemple
- IP Adress : Celle de votre machine distante
- DNS Name : Le nom DNS de la machine distante
![image](https://pierremonot.github.io/portfolio/zabbix-newhost.png)

5. Cliquer sur Add en bas à droite

6. Vérifier si les données remontent bien dans "Monitoring" -> "Latest Data"
