# System Update Role

Ce rôle Ansible gère les mises à jour système et configure les mises à jour automatiques.

## Fonctionnalités

- Force la synchronisation de l'heure système (systemd-timesyncd) **avant** les mises à jour
  - Évite les erreurs de certificats SSL liées à une horloge incorrecte
- Met à jour le cache APT
- Effectue une mise à jour complète du système (dist-upgrade)
- Installe et configure unattended-upgrades pour les mises à jour automatiques de sécurité

## Variables

Les variables suivantes peuvent être configurées dans `defaults/main.yml` :

### Unattended Upgrades

- `unattended_upgrades_origins`: Liste des origines de paquets à mettre à jour automatiquement
- `unattended_upgrades_auto_fix_interrupted_dpkg`: Réparer automatiquement les installations dpkg interrompues (défaut: true)
- `unattended_upgrades_minimal_steps`: Découper les mises à jour en petits morceaux (défaut: true)
- `unattended_upgrades_install_on_shutdown`: Installer lors de l'arrêt du système (défaut: false)
- `unattended_upgrades_mail`: Adresse email pour les notifications (défaut: "")
- `unattended_upgrades_mail_report`: Quand envoyer les rapports: "always", "only-on-error" ou "on-change" (défaut: "on-change")
- `unattended_upgrades_remove_unused_kernel_packages`: Supprimer les anciens noyaux (défaut: true)
- `unattended_upgrades_remove_unused_dependencies`: Supprimer les dépendances inutilisées (défaut: true)
- `unattended_upgrades_automatic_reboot`: Redémarrer automatiquement si nécessaire (défaut: false)
- `unattended_upgrades_automatic_reboot_time`: Heure du redémarrage automatique (défaut: "02:00")

### Auto Upgrades

- `auto_upgrades_update_package_lists`: Mise à jour de la liste des paquets (défaut: "1")
- `auto_upgrades_unattended_upgrade`: Activer les mises à jour automatiques (défaut: "1")
- `auto_upgrades_auto_clean_interval`: Intervalle de nettoyage en jours (défaut: "7")
- `auto_upgrades_download_upgradeable_packages`: Télécharger les paquets à l'avance (défaut: "1")

## Exemple d'utilisation

```yaml
- hosts: all
  become: yes
  roles:
    - system_update
```

Avec des variables personnalisées :

```yaml
- hosts: all
  become: yes
  roles:
    - role: system_update
      vars:
        unattended_upgrades_automatic_reboot: true
        unattended_upgrades_automatic_reboot_time: "03:00"
        unattended_upgrades_mail: "admin@example.com"
```

## Dépendances

Aucune

## Licence

MIT
