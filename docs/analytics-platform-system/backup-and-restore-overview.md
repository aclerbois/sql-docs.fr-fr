---
title: Sauvegarde et restauration - Parallel Data Warehouse | Documents Microsoft
description: Décrit comment les données de sauvegarde et restauration fonctionne pour Parallel Data Warehouse (PDW). Opérations de sauvegarde et de restauration sont utilisées pour la récupération d’urgence. Sauvegarde et restauration peuvent également servir à copier une base de données à partir d’une application sur un autre matériel.
author: mzaman1
manager: craigg
ms.prod: sql
ms.technology: data-warehouse
ms.topic: conceptual
ms.date: 04/17/2018
ms.author: murshedz
ms.reviewer: martinle
ms.openlocfilehash: 118b9ced12e01ac6655d85969bb61717f2b31e0b
ms.sourcegitcommit: 056ce753c2d6b85cd78be4fc6a29c2b4daaaf26c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/19/2018
---
# <a name="backup-and-restore"></a>Sauvegarde et restauration
Décrit comment les données de sauvegarde et restauration fonctionne pour Parallel Data Warehouse (PDW). Opérations de sauvegarde et de restauration sont utilisées pour la récupération d’urgence. Sauvegarde et restauration peuvent également servir à copier une base de données à partir d’une application sur un autre matériel.  
    
## <a name="BackupRestoreBasics"></a>Principes élémentaires de sauvegarde et restauration  
Un PDW *sauvegarde de base de données* est une copie d’une base de données d’appliance, stockée dans un format afin qu’il peut être utilisé pour restaurer la base de données d’origine vers un appareil.  
  
Création d’une sauvegarde de base de données PDW avec la [sauvegarde de base de données](../t-sql/statements/backup-database-parallel-data-warehouse.md) instruction t-sql et mis en forme pour une utilisation avec le [restaurer la base de données](../t-sql/statements/restore-database-parallel-data-warehouse.md) instruction ; il est inutilisable à d’autres fins. La sauvegarde peut uniquement être restaurée vers un appareil avec le même nombre ou un plus grand nombre de nœuds de calcul.  
  
<!-- MISSING LINKS

The [master database](master-database.md) is a SMP SQL Server database. It is backed up with the BACKUP DATABASE statement. To restore master, use the [Restore the Master Database](configuration-manager-restore-master-database.md) page of the Configuration Manager tool.  

-->
  
PDW utilise la technologie de sauvegarde de SQL Server pour sauvegarder et restaurer des bases de données de matériel. Options de sauvegarde de SQL Server sont préconfigurées pour utiliser la compression de la sauvegarde. Vous ne pouvez pas définir des options de sauvegarde comme la compression, la somme de contrôle, la taille des blocs ou le nombre de tampons.  
  
Sauvegardes de base de données sont stockées sur un ou plusieurs serveurs de sauvegarde, qui existe dans votre propre réseau client.  PDW écrit une sauvegarde de base de données utilisateur, en parallèle, directement à partir des nœuds de calcul à un serveur de sauvegarde et restaure une sauvegarde de base de données utilisateur en parallèle directement à partir de la sauvegarde du serveur pour les nœuds de calcul.  
  
Les sauvegardes sont stockées sur le serveur de sauvegarde comme un ensemble de fichiers dans le système de fichiers Windows. Une sauvegarde de base de données PDW peut uniquement être restaurée à PDW. Toutefois, vous pouvez archiver les sauvegardes de base de données à partir de la sauvegarde du serveur vers un autre emplacement à l’aide du processus de sauvegarde de fichier de Windows standard. Pour plus d’informations sur les serveurs de sauvegarde, consultez [Acquire et configurer un serveur de sauvegarde](acquire-and-configure-backup-server.md).  
  
## <a name="BackupTypes"></a>Types de sauvegarde de base de données  
Il existe deux types de données qui nécessitent une sauvegarde : bases de données utilisateur et les bases de données système (par exemple, la base de données master). PDW ne sauvegarde pas le journal des transactions.  
  
Une sauvegarde complète de la base de données est une sauvegarde d’une base de données entière PDW. Il s’agit du type de sauvegarde par défaut. Une sauvegarde complète de base de données utilisateur inclut la base de données et les rôles de base de données. Une sauvegarde de master comprend des connexions.  
  
Une sauvegarde différentielle contient toutes les modifications depuis la dernière sauvegarde complète. Généralement, une sauvegarde différentielle prend moins de temps qu’une sauvegarde complète et peut être effectuée plus souvent. Quand plusieurs sauvegardes différentielles sont basées sur la même sauvegarde complète, chaque sauvegarde différentielle inclut toutes les modifications dans la sauvegarde différentielle précédente.  
  
Par exemple, vous pouvez créer une sauvegarde complète hebdomadaire et une sauvegarde différentielle quotidienne. Pour restaurer la base de données utilisateur, de la sauvegarde complète ainsi que la dernière sauvegarde différentielle (le cas échéant) doit être restaurée.  
  
Une sauvegarde différentielle est uniquement pris en charge pour les bases de données utilisateur. Une sauvegarde de master est toujours une sauvegarde complète.  
  
Pour sauvegarder l’application entière, vous devez effectuer une sauvegarde de toutes les bases de données utilisateur et d’une sauvegarde de la base de données master.  
  
## <a name="BackupProc"></a>Processus de sauvegarde de base de données  
Le diagramme suivant illustre le flux de données lors d’une sauvegarde de base de données.  
  
![Processus de sauvegarde PDW](media/backup-process.png "les processus de sauvegarde PDW")  
  
Le processus de sauvegarde fonctionne comme suit :  
  
1.  Utilisateur soumet une instruction tsql de sauvegarde de base de données au nœud de contrôle.  
  
    -   La sauvegarde est une sauvegarde complète ou différentielle.  
  
2.  Pour les bases de données utilisateur, le nœud de contrôle (moteur de MPP) crée un plan de requête distribuée pour effectuer une sauvegarde de base de données parallèle.  
  
3.  Chaque nœud impliqué dans les copies de sauvegarde de son fichier de sauvegarde pour le serveur de sauvegarde à l’aide de la fonctionnalité de sauvegarde de SQL Server.  
  
    -   Chaque nœud impliqué copie un fichier de sauvegarde pour le serveur de sauvegarde.  
  
    -   La sauvegarde de base de données utilisateur (complète ou différentielle) inclut une sauvegarde de la partie de la base de données stocké sur chaque nœud de calcul et une sauvegarde des utilisateurs de base de données et des rôles de base de données.  
  
4.  La solution effectue la sauvegarde en parallèle à l’aide du réseau InfiniBand.  
  
    -   PDW effectue chaque sauvegarde complète et différentielle en parallèle. Toutefois, plusieurs sauvegardes de base de données ne sont pas exécutés simultanément. Chaque demande de sauvegarde doit attendre pour les sauvegardes précédemment soumis à la fin.  
  
    -   Une sauvegarde de la base de données master sauvegarde uniquement les données à partir du nœud de contrôle. Ce type de sauvegarde est effectué en série.  
  
5.  Une sauvegarde de base de données PDW est un groupe de fichiers stockés dans un répertoire qui se trouve hors de l’application. Le nom du répertoire est spécifié comme un nom de chemin d’accès et le répertoire réseau. Le répertoire ne peut pas être un chemin d’accès local, et il ne peut pas être sur le matériel.  
  
6.  Une fois la sauvegarde terminée, vous pouvez utiliser le système de fichiers Windows pour copier le répertoire de sauvegarde vers un autre emplacement, si vous le souhaitez.  
  
    -   Une sauvegarde peut être restaurée uniquement dans une appliance PDW qui a un nombre supérieur ou égal à égal de nœuds de calcul.  
  
    -   Vous ne pouvez pas modifier le nom de la sauvegarde avant d’effectuer une restauration. Le nom du répertoire de sauvegarde doit correspondre au nom d’origine de la sauvegarde. Le nom d’origine de la sauvegarde se trouve dans le fichier backup.xml dans le répertoire de sauvegarde. Pour restaurer une base de données à un autre nom, vous pouvez spécifier le nouveau nom dans la commande restore. Par exemple : `RESTORE DATABASE MyDB1 FROM DISK = ꞌ\\10.192.10.10\backups\MyDB2ꞌ`.  
  
## <a name="RestoreModes"></a>Modes de restauration de base de données  
Une restauration complète de la base de données recrée la base de données PDW en utilisant les données dans la sauvegarde de base de données. La restauration de la base de données est effectuée par tout d’abord en restaurant une sauvegarde complète, puis en restaurant une sauvegarde différentielle si vous le souhaitez. La restauration de la base de données inclut les utilisateurs de base de données et les rôles de base de données.  
  
Une restauration seul en-tête retourne les informations d’en-tête pour une base de données. Il ne restaure pas les données à l’application.  
  
Une restauration de l’application est une restauration de l’application entière. Cela inclut la restauration de toutes les bases de données utilisateur et la base de données master.  
  
## <a name="RestoreProc"></a>Processus de restauration  
Le diagramme suivant illustre le flux de données lors d’une restauration de base de données.  
  
![Processus de restauration](media/restore-process.png "processus de restauration")  
  
## <a name="restoring-to-an-appliance-with-the-same-number-of-compute-nodes"></a>La restauration sur un matériel possédant le même nombre de nœuds de calcul **  
  
Lors de la restauration des données, l’appliance détecte le nombre de nœuds de calcul sur l’appareil source et l’application de destination. Si les deux applications ont un nombre égal de nœuds de calcul, le processus de restauration fonctionne comme suit :  
  
1.  La sauvegarde de base de données à restaurer est disponible sur un partage de fichiers Windows sur un serveur de sauvegarde autre que l’appliance. Pour de meilleures performances, ce serveur est généralement connecté au réseau InfiniBand appliance.  
  
2.  Utilisateur soumet un [restaurer la base de données](../t-sql/statements/restore-database-parallel-data-warehouse.md) instruction tsql pour le nœud de contrôle.  
  
    -   La restauration est une restauration complète ou une restauration de l’en-tête. La restauration complète restaure une sauvegarde complète et restaure ensuite éventuellement une sauvegarde différentielle.  
  
3.  Le nœud de contrôle (moteur de MPP) crée un plan de requête distribuée pour effectuer une restauration de base de données parallèle.  
  
    -   SQL ServerPDW effectue la restauration d’une base de données utilisateur en parallèle. Toutefois, plusieurs sauvegardes de base de données et les restaurations ne sont pas exécutées simultanément. Le moteur MPP place chaque instruction de restauration dans une file d’attente ; Il doit attendre la sauvegarde précédemment soumise et les demandes à la fin de restauration.  
  
    -   Une restauration de la base de données master restaure uniquement les données vers le nœud de contrôle ; la restauration est effectuée en série.  
  
    -   Une restauration des informations d’en-tête est une opération rapide et ne restaure pas les données pour les nœuds de calcul ou un contrôle. Au lieu de cela, le nœud de contrôle retourne les résultats en tant que résultat de la requête.  
  
4.  Les fichiers de sauvegarde sont copiées vers les nœuds de calcul corrects en parallèle, généralement sur le réseau InfiniBand de matériel.  
  
5.  Chaque nœud de calcul restaure sa partie de la base de données utilisateur. Si un des restaurations ne se terminent pas correctement, toutes les bases de données sont supprimés et la restauration incorrectement.  
  
## <a name="restoring-to-an-appliance-with-a-larger-number-of-compute-nodes"></a>Restauration vers un appareil avec un plus grand nombre de nœuds de calcul  
  
Le fait de restaurer une sauvegarde dans une appliance dotée d’un plus grand nombre de nœuds de calcul a pour effet d’accroître la taille de la base de données allouée de façon proportionnelle au nombre de nœuds de calcul.  
  
Par exemple, lorsque vous restaurez une base de données de 60 Go à partir d’un équipement de 2 nœuds (30 Go par nœud) à un dispositif de 6 nœuds, SQL Server PDW crée une base de données 180 Go (6 nœuds avec 30 Go par nœud) sur le dispositif de 6 nœuds. SQL Server PDW initialement restaure la base de données à 2 nœuds correspondant à la configuration de la source, puis redistribue les données à tous les nœuds de 6.  
  
À l’issue de la redistribution, chaque nœud de calcul contient moins de données réelles et plus d’espace libre que chaque nœud de calcul de l’appliance source de plus petite taille. Profitez de l’espace supplémentaire pour ajouter davantage de données à la base de données. Si la taille de la base de données restaurée est plus grande que vous avez besoin, vous pouvez utiliser [ALTER DATABASE](../t-sql/statements/alter-database-parallel-data-warehouse.md) pour réduire la taille des fichiers de base de données.  
  
## <a name="related-tasks"></a>Tâches associées  
  
|Sauvegarde et la tâche de restauration| Description|  
|---------------------------|---------------|  
|Préparer un serveur en tant qu’un serveur de sauvegarde.|[Obtenir et configurer un serveur de sauvegarde ](acquire-and-configure-backup-server.md)|  
|Sauvegarde une base de données.|[BASE DE DONNÉES DE SAUVEGARDE](../t-sql/statements/backup-database-parallel-data-warehouse.md)|  
|Restaurer une base de données.|[RESTAURER LA BASE DE DONNÉES](../t-sql/statements/restore-database-parallel-data-warehouse.md)|    
<!-- MISSING LINKS
|Create a disaster recovery plan.|[Create a Disaster Recovery Plan](create-disaster-recovery-plan.md)|
|Restore the master database.|To restore the master database, use the [Restore the master database](configuration-manager-restore-master-database.md) page in the Configuration Manager tool.| 
|Copy a database from one appliance to another appliance.|[Copy a PDW database to another appliance](copy-pdw-database-to-another-appliance.md).|  
|Monitor backups and restores.|[Monitor backups and restores](monitor-backup-and-restore.md)|  
-->
