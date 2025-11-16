[Accueil](/index.md) | [Certifications](/certifications.md) | [Compétences](/competences.md) | [Projets](/projets.md) | [Scripts](/scripts.md)
# Zabbix - Installation
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

3. Installation Zabbix server, avec le frontend (l’interface Web pour le management) et l’agent
```
apt install zabbix-server-mysql zabbix-frontend-php zabbix-apache-conf zabbix-sql-scripts zabbix-agent
```
