---
title: Obtenir et configurer un serveur de sauvegarde - Parallel Data Warehouse | Documents Microsoft
description: Cet article décrit comment configurer un système de Windows sur le matériel non comme un serveur de sauvegarde pour une utilisation avec les fonctionnalités de sauvegarde et de restauration de système de plateforme Analytique (APS) et Parallel Data Warehouse (PDW).
author: mzaman1
manager: craigg
ms.prod: sql
ms.technology: data-warehouse
ms.topic: conceptual
ms.date: 04/17/2018
ms.author: murshedz
ms.reviewer: martinle
ms.openlocfilehash: 4464857e2b1e71a96f87e95d45df0577df987176
ms.sourcegitcommit: 056ce753c2d6b85cd78be4fc6a29c2b4daaaf26c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/19/2018
---
# <a name="acquire-and-configure-a-backup-server-for-parallel-data-warehouse"></a>Obtenir et configurer un serveur de sauvegarde pour Parallel Data Warehouse
Cet article décrit comment configurer un système de Windows sur le matériel non comme un serveur de sauvegarde pour une utilisation avec les fonctionnalités de sauvegarde et de restauration de système de plateforme Analytique (APS) et Parallel Data Warehouse (PDW).  
  
  
## <a name="Basics"></a>Principes de base de sauvegarde du serveur  
Le serveur de sauvegarde :  
  
-   Est fournie et géré par votre équipe informatique.  
  
-   Ne nécessite pas de n’importe quel logiciel PDW spécifique ou outils. PDW n’installe pas les logiciels sur le serveur de sauvegarde.  
  
-   Se trouve dans un rack de matériel non et ne peut pas être placé dans le dispositif de points d’accès.  
  
-   Peut être connecté au réseau InfiniBand appliance. Les sauvegardes peuvent être effectuées sur InfiniBand ou Ethernet ; InfiniBand est recommandée pour des raisons de performances.  
  
-   Se trouve dans votre propre domaine client, pas le domaine d’application. Il n’existe aucune relation d’approbation entre votre client et le domaine d’application.  
  
-   Héberge un partage de fichier de sauvegarde, qui est un partage de fichiers Windows qui utilise le protocole Server Message Block (SMB) au niveau de l’application. Les autorisations de partage de fichier de sauvegarde donner à un utilisateur de domaine Windows (cela n’est généralement un utilisateur de sauvegarde dédié) la possibilité de sauvegarder et restaurer des opérations sur le partage. Les informations d’identification de nom d’utilisateur et mot de passe de l’utilisateur de domaine Windows sont stockées dans PDW afin que PDW permettre sauvegarder et restaurer des opérations sur le partage de fichiers de sauvegarde.  
  
## <a name="Step1"></a>Étape 1 : Déterminer la capacité requise  
La configuration requise pour le serveur de sauvegarde dépend presque entièrement votre propre charge de travail. Avant l’acquisition ou de déploiement d’un serveur de sauvegarde, vous devez déterminer vos besoins en capacité. Le serveur de sauvegarde n’a pas à être dédié uniquement à des sauvegardes, tant qu’il gère les exigences de stockage et les performances de votre charge de travail. Vous pouvez également avoir plusieurs serveurs de sauvegarde pour sauvegarder et restaurer chaque base de données à une de plusieurs serveurs.  
  
Utilisez le [feuille de calcul Planification de la capacité de sauvegarde serveur](backup-capacity-planning-worksheet.md) pour aider à déterminer vos besoins en capacité.  
  
## <a name="Step2"></a>Étape 2 : Obtenir le serveur de sauvegarde  
Maintenant que vous comprenez mieux vos besoins en capacité, vous pouvez planifier les serveurs et les composants réseau dont vous aurez besoin pour l’achat ou de configurer. Incorporer la liste suivante des exigences de votre plan d’achat, puis achetez votre serveur ou configurer un serveur existant.  
  
### <a name="software-requirements"></a>Configuration logicielle requise  
N’importe quel serveur de fichiers qui utilise le protocole de partage de fichiers Windows (SMB).  
  
Nous vous recommandons de Windows Server 2012 ou au-delà de :  
  
-   Obtenir le gain de performances de préallocation de fichiers SMB.  
  
-   Utilisez l’initialisation de fichier instantanée (IFI) pour les opérations de sauvegarde. Votre équipe informatique gère ce paramètre sur le serveur de sauvegarde. Le Gestionnaire de Configuration PDW (dwconfig.exe) ne pas définir ou contrôler IFI sur votre serveur de sauvegarde. Les versions précédentes de Windows n’ont pas IFI, mais il peuvent toujours être utilisées en tant que serveurs de sauvegarde.  
  
### <a name="networking-requirements"></a>Configuration réseau requise  
Mais non obligatoire, InfiniBand est le type de connexion recommandé pour les serveurs de sauvegarde. Pour préparer la connexion le chargement du serveur au réseau InfiniBand appliance :  
  
1.  Envisagez le serveur en rack suffisamment proches pour l’application afin que vous pouvez vous connecter à l’appliance InfiniBand bascule. Pour plus d’informations à partir de Mellanox Technologies InfiniBand, voir le livre blanc, [présentation InfiniBand](http://www.mellanox.com/pdf/whitepapers/IB_Intro_WP_190.pdf).  
  
2.  Acheter une carte simple ou double port Mellanox ConnectX-3 Verification InfiniBand. Nous vous recommandons d’achat de la carte réseau avec deux ports pour la tolérance de panne pendant la transmission de données. Une carte réseau de deux ports est requise pour la haute disponibilité.  
  
3.  Acheter 2 câbles Verification InfiniBand pour une carte de deux ports ou 1 câble Verification InfiniBand pour une carte de port unique. Les câbles Verification InfiniBand seront connecte le chargement du serveur au réseau InfiniBand appliance. La longueur du câble dépend de la distance entre le serveur de chargement et les commutateurs InfiniBand du matériel, selon votre environnement.  
  
## <a name="Step3"></a>Étape 3 : Relier le serveur aux réseaux InfiniBand  
Utilisez ces étapes pour connecter le chargement du serveur au réseau InfiniBand. Si le serveur n’utilise pas le réseau InfiniBand, ignorez cette étape.  
  
1.  Rack serveur suffisamment proches pour l’application afin que vous pouvez vous connecter au réseau InfiniBand appliance.  
  
2.  Installer la carte réseau InfiniBand Mellanox ConnectX-3 Verification InfiniBand sur le serveur de chargement.  
  
3.  Utilisez les câbles de Verification pour se connecter la carte réseau InfiniBand à un des deux commutateurs InfiniBand dans le premier rack de matériel.  
  
4.  Installez et configurez le pilote Windows approprié pour la carte réseau InfiniBand.  
  
    -   Pilotes InfiniBand pour Windows sont développées par Alliance OpenFabrics, un consortium industriel InfiniBand fournisseurs.  Le pilote peut ont été distribué avec votre carte de réseau InfiniBand. Si ce n’est pas le cas, le pilote peut être téléchargé à partir de www.openfabrics.org.  
  
5.  Configurez les paramètres DNS et InfiniBand pour les cartes réseau. Pour obtenir des instructions de configuration, consultez [configurer des cartes réseau InfiniBand](configure-infiniband-network-adapters.md).  
  
## <a name="Step4"></a>Étape 4 : Configurer le partage de fichiers de sauvegarde  
PDW accéderont à la sauvegarde du serveur via un partage de fichiers UNC. Pour configurer le partage de fichiers :  
  
1.  Créez un dossier sur le serveur de sauvegarde pour stocker vos sauvegardes.  
  
2.  Créer un partage de fichiers, appelé un partage de sauvegarde pour le dossier de sauvegarde.  
  
3.  Désignez ou créez un compte de domaine Windows dans votre domaine de client que vous souhaitez utiliser dans le cadre de l’exécution de sauvegardes et restaurations. Pour des raisons de sécurité, il est préférable d’utiliser un compte dédié en tant que l’utilisateur de sauvegarde.  
  
4.  Ajouter des autorisations pour la sauvegarde partagent afin que seuls les comptes approuvés et un compte de sauvegarde du domaine puissent accéder, lire et écrire dans l’emplacement du partage.  
  
5.  Ajoutez les informations d’identification du compte de domaine à PDW.  
  
    Par exemple :  
  
    ```sql  
    EXEC sp_pdw_add_network_credentials '10.192.147.63', 'seattle\david', '********';  
    ```  
  
    Pour plus d’informations, consultez ces procédures stockées :  
  
    -   [sp_pdw_add_network_credentials](../relational-databases/system-stored-procedures/sp-pdw-add-network-credentials-sql-data-warehouse.md)  
  
    -   [sp_pdw_remove_network_credentials](../relational-databases/system-stored-procedures/sp-pdw-remove-network-credentials-sql-data-warehouse.md)  
  
## <a name="Step5"></a>Étape 5 : Commencez à sauvegarder vos données  
Vous êtes maintenant prêt à commencer la sauvegarde des données sur votre serveur de sauvegarde.  
  
Pour sauvegarder des données, utilisez un client de la requête pour se connecter à SQL Server PDW, puis soumettez sauvegarde de base de données ou de restauration de base de données des commandes. Utiliser le disque = clause pour spécifier l’emplacement de sauvegarde et de serveur de sauvegarde.  
  
> [!IMPORTANT]  
> N’oubliez pas d’utiliser l’adresse InfiniBand IP du serveur de sauvegarde. Dans le cas contraire, les données seront copiées sur Ethernet au lieu de InfiniBand.  
  
Par exemple :  
  
```sql  
BACKUP DATABASE Invoices TO DISK = '\\10.172.14.255\backups\yearly\Invoices2013Full';  
  
RESTORE DATABASE Invoices2013Full  
FROM DISK = '\\10.172.14.255\backups\yearly\Invoices2013Full'  
```  
  
Pour plus d'informations, consultez : 
  
-   [BASE DE DONNÉES DE SAUVEGARDE](../t-sql/statements/backup-database-parallel-data-warehouse.md)   
  
-   [RESTAURER LA BASE DE DONNÉES](../t-sql/statements/restore-database-parallel-data-warehouse.md)  
  
## <a name="Security"></a>Avis de sécurité  
Le serveur de sauvegarde n’est pas joint au domaine privé de l’application. Il est dans votre propre réseau, et il n’existe aucune relation d’approbation entre votre propre domaine et le domaine du matériel privé.  
  
Étant donné que les sauvegardes PDW ne sont pas stockés sur le matériel, votre équipe informatique est chargé de gérer tous les aspects de la sécurité de la sauvegarde. Par exemple, cela inclut la gestion de la sécurité de la sécurité de l’infrastructure réseau qui connecte le serveur de sauvegarde à l’application des points d’accès, la sécurité du serveur utilisé pour stocker les sauvegardes et les données de sauvegarde.  
  
### <a name="manage-network-credentials"></a>Gérer les informations d’identification réseau  
  
L’accès réseau au répertoire de sauvegarde est basé sur la sécurité standard des partages de fichiers Windows. Avant d’effectuer une sauvegarde, vous devez créer ou désigner un compte Windows qui sera utilisé pour l’authentification PDW dans le répertoire de sauvegarde. Ce compte Windows doit être doté d’un droit d’accès, de création et d’écriture sur le répertoire de sauvegarde.  
  
> [!IMPORTANT]  
> Pour réduire les risques de sécurité liés à vos données, il vous est conseillé de désigner un compte Windows qui servira uniquement aux opérations de sauvegarde et de restauration. Autorisez ce compte à accéder à l’emplacement de sauvegarde et à aucun autre emplacement.  
  
Pour stocker le nom d’utilisateur et un mot de passe dans PDW, utilisez le [sp_pdw_add_network_credentials](../relational-databases/system-stored-procedures/sp-pdw-add-network-credentials-sql-data-warehouse.md) procédure stockée. PDW utilise le Gestionnaire d’informations d’identification Windows pour stocker et de chiffrer des noms d’utilisateur et mots de passe sur le nœud de contrôle et de nœuds de calcul. Les informations d’identification ne sont pas sauvegardées avec la commande BACKUP DATABASE.  
  
Pour supprimer les informations d’identification réseau de PDW, utilisez le [sp_pdw_remove_network_credentials](../relational-databases/system-stored-procedures/sp-pdw-remove-network-credentials-sql-data-warehouse.md) procédure stockée.  
  
Pour répertorier toutes les informations d’identification réseau stockées dans SQL Server PDW, utilisez le [sys.dm_pdw_network_credentials](../relational-databases/system-dynamic-management-views/sys-dm-pdw-network-credentials-transact-sql.md) vue de gestion dynamique.  
  
### <a name="secure-communications"></a>Communications sécurisées  
  
Opérations sur le serveur de chargement peuvent utiliser un chemin d’accès UNC pour extraire des données à partir de l’extérieur du réseau interne approuvé. Une personne malveillante sur le réseau ou avec la possibilité pour influencer la résolution de noms peut intercepter ou modifier les données envoyées à l’empaquetage. Cela pose un risque de divulgation de falsification et informations. Pour aider à atténuer le risque de falsification :

- Exiger la signature sur la connexion. 
- Sur le serveur de chargement, définissez l’option de stratégie de groupe suivant de sécurité\Stratégies Locales\options de sécurité : client réseau Microsoft : communications signées numériquement (toujours) : activé.  
  
## <a name="see-also"></a>Voir aussi  
[Sauvegarde et restauration](backup-and-restore-overview.md)  
  
