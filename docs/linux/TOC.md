# [À propos de SQL Server sur Linux](sql-server-linux-overview.md)

# Vue d'ensemble
## [Notes de publication](sql-server-linux-release-notes.md)
## [Nouveautés](sql-server-linux-whats-new.md)
## [nouvelles articles et articles mises à jour](new-updated-linux.md)
## [Éditions et fonctionnalités prises en charge](sql-server-linux-editions-and-components-2017.md)
## [Questions fréquentes (FAQ)](sql-server-linux-faq.md)

# Démarrages rapides
## [Installer et connecter - Red Hat](quickstart-install-connect-red-hat.md)
## [Installer et connecter - SUSE](quickstart-install-connect-suse.md)
## [Installer et connecter - Ubuntu](quickstart-install-connect-ubuntu.md)
## [Exécuter et connecter - Docker](quickstart-install-connect-docker.md)
## [Approvisionner une machine virtuelle SQL dans Azure](/azure/virtual-machines/linux/sql/provision-sql-server-linux-virtual-machine?toc=%2fsql%2flinux%2ftoc.json)
## [Exécuter et connecter - Cloud](quickstart-install-connect-clouds.md)

# Didacticiels
## [1_Migrer depuis Windows](sql-server-linux-migrate-restore-database.md)
## [2_Migrer depuis Oracle](../ssma/oracle/sql-server-linux-convert-from-oracle.md?toc=%2fsql%2flinux%2ftoc.json)
## [3_Migrer vers Docker](tutorial-restore-backup-in-sql-server-container.md)
## [4_Créer une tâche](sql-server-linux-run-sql-server-agent-job.md)
## [5_Configurer l’authentification AD](sql-server-linux-active-directory-authentication.md)
## [6_Créer une instance de cluster de basculement](sql-server-linux-shared-disk-cluster-configure.md)
### [iSCSI](sql-server-linux-shared-disk-cluster-configure-iscsi.md)
### [NFS](sql-server-linux-shared-disk-cluster-configure-nfs.md)
### [SMB](sql-server-linux-shared-disk-cluster-configure-smb.md)
## [7_Déployer un cluster Pacemaker](sql-server-linux-deploy-pacemaker-cluster.md)
## [8_Créer et configurer les groupes de disponibilité](sql-server-linux-create-availability-group.md)
## [9_Configurer dans Kubernetes pour la haute disponibilité](tutorial-sql-server-containers-kubernetes.md)

# Concepts
## Install
### [Installer SQL Server](sql-server-linux-setup.md)
### [Installer les outils SQL Server](sql-server-linux-setup-tools.md)
### [Installer SQL Server Agent](sql-server-linux-setup-sql-agent.md)
### [Installer la recherche en texte intégral SQL Server](sql-server-linux-setup-full-text-search.md)
### [Installer SQL Server Integration Services](sql-server-linux-setup-ssis.md)
### [Configurer les référentiels](sql-server-linux-change-repo.md)

## Configurer
### [Configurer avec mssql-conf](sql-server-linux-configure-mssql-conf.md)
### [Variables d’environnement](sql-server-linux-configure-environment-variables.md)
### [Configurer des conteneurs Docker](sql-server-linux-configure-docker.md)
### [Commentaires client](sql-server-linux-customer-feedback.md)

## [Développer](sql-server-linux-develop-overview.md)
### [Bibliothèques de connectivité](sql-server-linux-develop-connectivity-libraries.md)
### [Utilisez Visual Studio Code](sql-server-linux-develop-use-vscode.md)
### [Utiliser SSDT](sql-server-linux-develop-use-ssdt.md)

## [Gérer](sql-server-linux-management-overview.md)
### [Utilisez SSMS pour gérer](sql-server-linux-manage-ssms.md)
### [Utiliser PowerShell pour gérer](sql-server-linux-manage-powershell.md)
### [Utiliser la copie des journaux de transaction](sql-server-linux-use-log-shipping.md)
### [Utiliser la messagerie de base de données et les alertes de messagerie](sql-server-linux-db-mail-sql-agent.md)

## [Migration](sql-server-linux-migrate-overview.md)
### [Exporter et importer un fichier BACPAC à partir de Windows](sql-server-linux-migrate-ssms.md)
### [Migrer avec SQL Server Migration Assistant](sql-server-linux-migrate-ssma.md)
### [Copie en bloc avec bcp](sql-server-linux-migrate-bcp.md)

## [Extraire, transformer, charger](sql-server-linux-migrate-ssis.md)
### [Limitations et problèmes connus](sql-server-linux-ssis-known-issues.md)
### [Configurer SSIS](sql-server-linux-configure-ssis.md)
### [Planifier des packages SSIS](sql-server-linux-schedule-ssis-packages.md)

## [Configurer la continuité de l’activité](sql-server-linux-business-continuity-dr.md)
### [Notions de base sur la disponibilité](sql-server-linux-ha-basics.md)
### [Sauvegarde et restauration](sql-server-linux-backup-and-restore-database.md)
#### [Interface d’appareil virtuel - Linux](sql-server-linux-backup-vdi-specification.md)
### [Instance de cluster de basculement](sql-server-linux-shared-disk-cluster-concepts.md)
#### [Red Hat Enterprise Linux (RHEL)]()
##### [Configurer (extension de haute disponibilité)](sql-server-linux-shared-disk-cluster-red-hat-7-configure.md)
##### [Opérer (extension de haute disponibilité)](sql-server-linux-shared-disk-cluster-red-hat-7-operate.md)
#### [SUSE Linux Enterprise Server (SLES)]()
##### [Configurer (extension de haute disponibilité)](sql-server-linux-shared-disk-cluster-sles-configure.md)
### [Groupes de disponibilité](sql-server-linux-availability-group-overview.md)
#### [Créer pour une haute disponibilité](sql-server-linux-availability-group-ha.md)
##### [Configurer un groupe de disponibilité](sql-server-linux-availability-group-configure-ha.md)
##### [Configurer sur RHEL](sql-server-linux-availability-group-cluster-rhel.md)
##### [Configurer sur SLES](sql-server-linux-availability-group-cluster-sles.md)
##### [Configurer sur Ubuntu](sql-server-linux-availability-group-cluster-ubuntu.md)
##### [Basculement](sql-server-linux-availability-group-failover-ha.md)
##### [Opérer](sql-server-linux-availability-group-operate-ha.md)
##### [Configurer plusieurs sous-réseaux pour la disponibilité](sql-server-linux-configure-multiple-subnet.md)
#### [Créer pour l’échelle lecture uniquement]()
##### [Configurer un groupe de disponibilité](sql-server-linux-availability-group-configure-rs.md)
#### [Configurer la fonctionnalité multiplateforme (Windows et Linux)](sql-server-linux-availability-group-cross-platform.md)

## [Sécurité](sql-server-linux-security-overview.md)
### [Bien démarrer avec les fonctionnalités de sécurité](sql-server-linux-security-get-started.md)
### [Authentification Active Directory](sql-server-linux-active-directory-auth-overview.md)
### [Chiffrement des connexions](sql-server-linux-encrypted-connections.md)

## Performances
### [Bonnes pratiques](sql-server-linux-performance-best-practices.md)
### [Bien démarrer avec les fonctionnalités relatives aux performances](sql-server-linux-performance-get-started.md)

# Exemples
## Installation sans assistance
### [Red Hat Enterprise Linux (RHEL)](sample-unattended-install-redhat.md)
### [SUSE Linux Enterprise Server (SLES)](sample-unattended-install-suse.md)
### [Ubuntu](sample-unattended-install-ubuntu.md)

# Ressources
## [Dépanner](sql-server-linux-troubleshooting-guide.md)
## [Documentation SQL Server](../sql-server/sql-server-technical-documentation.md)
## Partenaires
### [Surveillance](../sql-server/partner-monitor-sql-server.md)
### [Haute disponibilité et récupération d’urgence](../sql-server/partner-hadr-sql-server.md)
### [Gestion](../sql-server/partner-management-sql-server.md)
### [Développement](../sql-server/partner-dev-sql-server.md)
## [DBA Stack Exchange](https://dba.stackexchange.com/questions/tagged/sql-server)
## [Stack Overflow](http://stackoverflow.com/questions/tagged/sql-server)
## [Forums MSDN](https://social.msdn.microsoft.com/Forums/en-US/home?category=sqlserver)
## [Envoyer vos commentaires :](https://feedback.azure.com/forums/908035-sql-server)
## [Reddit](https://www.reddit.com/r/SQLServer)