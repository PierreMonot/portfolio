```
---
- name: Maintenance et mise à jour des serveurs Debian et Ubuntu
  hosts: all
  become: true
  gather_facts: true

  vars:
    # Comportement configurable
    installer_unattended: false     # true pour installer unattended-upgrades
    reboot_if_needed: true          # redémarrer si un reboot est requis
    autoremove: true                # suppression paquets obsolètes
    upgrade_type: "full-upgrade"    # "upgrade" ou "full-upgrade"
    check_mode_safe: false          # true pour arrêter le play avant d'appliquer
    max_reboot_wait: 300            # timeout reboot (secondes)

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

    - name: Vérifier l'existence du fichier indiquant qu'un redémarrage est requis
      stat:
        path: /var/run/reboot-required
      register: reboot_required_file
      when: ansible_facts['os_family'] == "Debian"
      tags: reboot-check

    - name: Définir le fait reboot_required
      set_fact:
        reboot_required: "{{ reboot_required_file.stat.exists | default(false) }}"
      when: ansible_facts['os_family'] == "Debian"
      tags: reboot-check

    - name: Redémarrer l'hôte si nécessaire
      reboot:
        reboot_timeout: "{{ max_reboot_wait }}"
        test_command: whoami
      when: reboot_if_needed and reboot_required
      tags: reboot
```
