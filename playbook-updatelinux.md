[Accueil](index.md) · [Certifications](certifications.md) · [Compétences](competences.md) · [Projets](projets.md) · [Scripts](scripts.md)
 
# Linux - Playbook de mise à jour de l'OS
 
**Contexte** : utilisé sur mon homelab, qui compte plusieurs VMs Debian et Ubuntu.
 
**Objectif** : mettre à jour tous mes serveurs Linux en une seule commande, au lieu de me connecter sur chacun. Un peu comme WSUS centralise les mises à jour pour Windows.
 
## Ce que fait le playbook
 
1. Met à jour le cache APT (sauf s'il a moins d'une heure)
2. Met à niveau les paquets
3. Supprime les paquets devenus inutiles
4. Redémarre le serveur seulement si une mise à jour l'exige

## Le playbook
 
{% raw %}
```yaml
---
- name: Maintenance et mise à jour des serveurs Debian et Ubuntu
  hosts: all
  become: true
  gather_facts: true
 
  vars:
    reboot_if_needed: true    # redémarrer si un reboot est requis
    autoremove: true          # supprimer les paquets obsolètes
    upgrade_type: "dist"      # "safe" (apt upgrade) ou "dist" (apt full-upgrade)
    max_reboot_wait: 300      # délai maximal du redémarrage (secondes)
 
  tasks:
 
    - name: Mettre à jour le cache APT
      apt:
        update_cache: yes
        cache_valid_time: 3600
      when: ansible_facts['os_family'] == "Debian"
      tags: update-cache
 
    - name: Mettre à niveau les paquets APT
      apt:
        upgrade: "{{ upgrade_type }}"
      when: ansible_facts['os_family'] == "Debian"
      register: resultat_upgrade
      tags: upgrade
 
    - name: Supprimer et nettoyer les paquets APT obsolètes
      apt:
        autoremove: yes
        autoclean: yes
      when: autoremove and ansible_facts['os_family'] == "Debian"
      tags: cleanup
 
    - name: Vérifier si un redémarrage est requis
      stat:
        path: /var/run/reboot-required
      register: reboot_required_file
      when: ansible_facts['os_family'] == "Debian"
      tags: reboot-check
 
    - name: Redémarrer l'hôte si nécessaire
      reboot:
        reboot_timeout: "{{ max_reboot_wait }}"
        test_command: whoami
      when:
        - reboot_if_needed
        - reboot_required_file.stat.exists | default(false)
      tags: reboot
```
{% endraw %}
 
## Utilisation
 
Tester d'abord sans rien modifier :
 
```bash
ansible-playbook -i inventaire.ini maj-linux.yml --check
```
 
Lancer la mise à jour :
 
```bash
ansible-playbook -i inventaire.ini maj-linux.yml
```