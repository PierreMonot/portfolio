[Accueil](/index.md) | [Certifications](/certifications.md) | [Compétences](/competences.md) | [Projets](/projets.md) | [Scripts](/scripts.md)
# Zabbix 7.4 - Installation Agent (Linux)

1. Se mettre en root
```
sudo su 
```

2. Télécharger/extraire le .deb du repo Zabbix 
```
wget https://repo.zabbix.com/zabbix/7.4/release/ubuntu/pool/main/z/zabbix-release/zabbix-release_latest_7.4+ubuntu24.04_all.deb
dpkg -i zabbix-release_latest_7.4+ubuntu24.04_all.deb
apt update 
```

3. Installation de l'agent
```
apt install zabbix-agent2
```


4. Modifier le fichier au chemin /etc/zabbix/zabbix_agent2.conf
```
Server=IPduServeurZabbix
```

5. Redémarrer le service et le démarrer automatiquement au démarrage de l'OS
```
systemctl restart zabbix-agent2
systemctl enable zabbix-agent2
```
