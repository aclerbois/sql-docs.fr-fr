---
title: Références relatives aux erreurs et aux événements (réplication) | Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: replication
ms.reviewer: ''
ms.suite: sql
ms.technology:
- replication
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- errors [SQL Server replication], troubleshooting
- troubleshooting [SQL Server replication], errors
- replication [SQL Server], troubleshooting
- errors [SQL Server replication]
- errors and events reference [SQL Server replication]
ms.assetid: e67d1bab-47b6-441d-ab9c-251a2ca499e1
caps.latest.revision: 25
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: 484dca1df1d644799bc64bc552c9648029129d76
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="errors-and-events-reference-replication"></a>Guide de référence des erreurs et des événements (réplication)
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Cette section de la documentation contient des informations sur la cause et la résolution de nombreuses erreurs liées à la réplication.  
  
|Error|Message|  
|-----------|-------------|  
|[MSSQL_ENG002601](../../relational-databases/replication/mssql-eng002601.md)|Impossible d’insérer une ligne de clé en double dans l’objet '%1!' avec un index unique '%.\*ls'.|  
|[MSSQL_ENG002627](../../relational-databases/replication/mssql-eng002627.md)|Violation de %1! contrainte '%2!'. Impossible d’insérer une clé en double dans l’objet '%.\*ls'.|  
|[MSSQL_ENG003165](../../relational-databases/replication/mssql-eng003165.md)|La base de données '%ls' a été restaurée ; cependant, une erreur est survenue lors de la restauration/suppression de la réplication. La base de données est restée hors ligne. Consultez la rubrique MSSQL_ENG003165 dans la documentation en ligne de SQL Server.|  
|[MSSQL_ENG003724](../../relational-databases/replication/mssql-eng003724.md)|Impossible de %1! le %2! '%3!' parce qu'il est utilisé pour la réplication.|  
|[MSSQL_ENG004929](../../relational-databases/replication/mssql-eng004929.md)|Impossible de modifier %1! '%2!' parce qu'elle est en cours d'édition pour la réplication.|  
|MSSQL_ENG007395. Consultez [Troubleshooting Oracle Publishers](../../relational-databases/replication/non-sql/troubleshooting-oracle-publishers.md).|Impossible pour le fournisseur OLE DB « %ls » du serveur lié « %ls » de démarrer une transaction imbriquée. Cette dernière est obligatoire, car l'option XACT_ABORT est définie à la valeur OFF.|  
|[MSSQL_ENG014005](../../relational-databases/replication/mssql-eng014005.md)|Impossible de supprimer la publication. Il existe un abonnement.|  
|[MSSQL_ENG014010](../../relational-databases/replication/mssql-eng014010.md)|Le serveur '%s' n'est pas défini comme serveur d'abonnements.|  
|[MSSQL_ENG014114](../../relational-databases/replication/mssql-eng014114.md)|'%1!' n'est pas configuré en tant que distributeur.|  
|[MSSQL_ENG014117](../../relational-databases/replication/mssql-eng014117.md)|'%1!' n'est pas configuré comme base de données de distribution.|  
|[MSSQL_ENG014120](../../relational-databases/replication/mssql-eng014120.md)|Impossible de supprimer la base de données de distribution '%s'. La base de données de distribution est associée à un éditeur.|  
|[MSSQL_ENG014121](../../relational-databases/replication/mssql-eng014121.md)|Impossible de supprimer le distributeur '%s'. Des bases de données de distribution sont associées à ce distributeur.|  
|[MSSQL_ENG014144](../../relational-databases/replication/mssql-eng014144.md)|Impossible de supprimer l’Abonné '%s'. La base de données de publication '%s' comporte des abonnements qui lui sont associés.|  
|[MSSQL_ENG014150](../../relational-databases/replication/mssql-eng014150.md)|Réplication-%s : réussite de l'agent %s. %s|  
|[MSSQL_ENG014151](../../relational-databases/replication/mssql-eng014151.md)|Réplication-%s : échec de l'agent %s. %s|  
|[MSSQL_ENG014152](../../relational-databases/replication/mssql-eng014152.md)|Réplication-%s : agent %s programmé pour réessayer. %s|  
|[MSSQL_ENG014157](../../relational-databases/replication/mssql-eng014157.md)|L'abonnement créé par l'Abonné '%1!' à la publication '%2!' a expiré et a été supprimé.|  
|[MSSQL_ENG014160](../../relational-databases/replication/mssql-eng014160.md)|Le seuil [%s:%s] de la publication [%s] a été défini. Un ou plusieurs abonnements à cette publication ont expiré.|  
|[MSSQL_ENG014161](../../relational-databases/replication/mssql-eng014161.md)|Le seuil [%s:%s] de la publication [%s] a été défini. Assurez-vous que le lecteur du journal et les agents de distribution s'exécutent et peuvent respecter les conditions de latence.|  
|[MSSQL_ENG014162](../../relational-databases/replication/mssql-eng014162.md)|Le seuil [%s:%s] de la publication [%s] a été défini. Assurez-vous que l'Agent de fusion s'exécute et peut respecter les conditions de latence.|  
|[MSSQL_ENG014163](../../relational-databases/replication/mssql-eng014163.md)|Le seuil [%s:%s] de la publication [%s] a été défini. Assurez-vous que l'Agent de fusion s'exécute et peut respecter les conditions de latence.|  
|[MSSQL_ENG014164](../../relational-databases/replication/mssql-eng014164.md)|Le seuil [%s:%s] de la publication [%s] a été défini. Assurez-vous que l'Agent de fusion s'exécute et peut respecter les conditions de latence.|  
|[MSSQL_ENG014165](../../relational-databases/replication/mssql-eng014165.md)|Le seuil [%s:%s] de la publication [%s] a été défini. Assurez-vous que l'Agent de fusion s'exécute et peut respecter les conditions de latence.|  
|[MSSQL_ENG018456](../../relational-databases/replication/mssql-eng018456.md)|Échec de la connexion pour l’utilisateur '%.*ls'.%.\*ls|  
|[MSSQL_ENG018752](../../relational-databases/replication/mssql-eng018752.md)|Un seul Agent de lecture du journal ou une seule procédure liée au journal (sp_repldone, sp_replcmds et sp_replshowcmds) peut se connecter à une base de données à la fois. Si vous avez exécuté une procédure liée au journal, supprimez la connexion à travers laquelle fut exécutée la procédure ou exécutez sp_replflush sur cette connexion avant de démarrer l'Agent de lecture du journal ou d'exécuter toute autre procédure liée au journal.|  
|[MSSQL_ENG020554](../../relational-databases/replication/mssql-eng020554.md)|L'agent de réplication n'a enregistré aucun message d'état d'avancement en %ld minutes. Il se peut que l'agent ne réponde pas ou que l'activité du système soit élevée. Vérifiez que les enregistrements sont répliqués vers la destination et que les connexions à l'Abonné, au serveur de publication et au serveur de distribution sont toujours actives.|  
|[MSSQL_ENG020557](../../relational-databases/replication/mssql-eng020557.md)|Arrêt de l'Agent. Pour plus d'informations, reportez-vous à l'historique des travaux de l'Agent SQL Server pour le travail « %s ».|  
|[MSSQL_ENG020572](../../relational-databases/replication/mssql-eng020572.md)|L'Abonné '%s' avec un abonnement à l'article '%s' de la publication '%s' a été réinitialisé après l'échec de la validation.|  
|[MSSQL_ENG020574](../../relational-databases/replication/mssql-eng020574.md)|L'Abonné '%s' avec un abonnement à l'article '%s' de la publication '%s' n'a pas réussi la validation de données.|  
|[MSSQL_ENG020575](../../relational-databases/replication/mssql-eng020575.md)|L'Abonné '%s' avec un abonnement à l'article '%s' de la publication '%s' a passé la validation de données.|  
|[MSSQL_ENG020596](../../relational-databases/replication/mssql-eng020596.md)|Seul '%1!' ou les membres de db_owner peuvent supprimer l'Agent anonyme.|  
|[MSSQL_ENG020598](../../relational-databases/replication/mssql-eng020598.md)|La ligne n'a pas été trouvée chez l'Abonné lorsque la commande répliquée est appliquée.|  
|[MSSQL_ENG021075](../../relational-databases/replication/mssql-eng021075.md)|L'instantané initial de la publication '%1!' n'est pas encore disponible.|  
|[MSSQL_ENG021076](../../relational-databases/replication/mssql-eng021076.md)|L'instantané initial de l'article '%1!' n'est pas encore disponible.|  
|[MSSQL_ENG021286](../../relational-databases/replication/mssql-eng021286.md)|La table de conflits '%1!' n'existe pas.|  
|[MSSQL_ENG021330](../../relational-databases/replication/mssql-eng021330.md)|Impossible de créer un sous-répertoire dans le répertoire de travail de la réplication.(%1!)|  
|[MSSQL_ENG021331](../../relational-databases/replication/mssql-eng021331.md)|Impossible de copier le fichier de script utilisateur vers le distributeur.(%1!)|  
|[MSSQL_ENG021385](../../relational-databases/replication/mssql-eng021385.md)|L'instantané n'a pas réussi à traiter la publication '%1!'. L'incident peut être dû à une activité de changement de schéma actif ou à l'ajout de nouveaux articles.|  
|MSSQL_ENG021617. Consultez [Troubleshooting Oracle Publishers](../../relational-databases/replication/non-sql/troubleshooting-oracle-publishers.md).|Impossible d'exécuter SQL*PLUS. Assurez-vous qu'une version actuelle du code client Oracle est installée sur le serveur de distribution.|  
|MSSQL_ENG021620. Consultez [Troubleshooting Oracle Publishers](../../relational-databases/replication/non-sql/troubleshooting-oracle-publishers.md).|La version de SQL*PLUS qui est accessible via la variable système Path n'est pas assez récente pour prendre en charge la publication dans Oracle. Assurez-vous qu'une version actuelle du code client Oracle est installée sur le serveur de distribution.|  
|MSSQL_ENG021624. Consultez [Troubleshooting Oracle Publishers](../../relational-databases/replication/non-sql/troubleshooting-oracle-publishers.md).|Impossible de trouver le fournisseur Oracle OLEDB enregistré, OraOLEDB.Oracle, sur le serveur de distribution '%s'. Assurez-vous qu'une version actuelle du fournisseur Oracle OLEDB est installée et enregistrée sur le serveur de distribution.|  
|MSSQL_ENG021626. Consultez [Troubleshooting Oracle Publishers](../../relational-databases/replication/non-sql/troubleshooting-oracle-publishers.md).|Impossible de se connecter au serveur de base de données Oracle '%s' à l'aide du fournisseur Oracle OLEDB OraOLEDB.Oracle.|  
|MSSQL_ENG021627. Consultez [Troubleshooting Oracle Publishers](../../relational-databases/replication/non-sql/troubleshooting-oracle-publishers.md).|Impossible de se connecter au serveur de base de données Oracle '%1!s!' au moyen du fournisseur Microsoft OLEDB MSDAORA.|  
|MSSQL_ENG021628. Consultez [Troubleshooting Oracle Publishers](../../relational-databases/replication/non-sql/troubleshooting-oracle-publishers.md).|Impossible de mettre à jour le Registre du serveur de distribution '%s' pour permettre au fournisseur Oracle OLEDB OraOLEDB.Oracle de s'exécuter avec SQL Server. Vérifiez que la connexion actuelle est autorisée à modifier les clés de Registre dont SQL Server est propriétaire.|  
|MSSQL_ENG021629. Consultez [Troubleshooting Oracle Publishers](../../relational-databases/replication/non-sql/troubleshooting-oracle-publishers.md).|La clé de Registre CLSID indiquant que le fournisseur Oracle OLEDB pour Oracle, OraOLEDB.Oracle, a été enregistré ne se trouve pas sur le serveur de distribution. Assurez-vous que le fournisseur Oracle OLEDB est installé et enregistré sur le serveur de distribution.|  
|MSSQL_ENG021642. Consultez [Troubleshooting Oracle Publishers](../../relational-databases/replication/non-sql/troubleshooting-oracle-publishers.md).|Les serveurs de publication hétérogènes requièrent un serveur lié. Un serveur lié appelé '%s' existe déjà. Supprimez ce serveur ou choisissez un autre nom de serveur de publication.|  
|MSSQL_ENG021663. Consultez [Troubleshooting Oracle Publishers](../../relational-databases/replication/non-sql/troubleshooting-oracle-publishers.md).|Aucune clé primaire valide n'a été trouvée pour la table source [%s].[%s].|  
|MSSQL_ENG021684. Consultez [Troubleshooting Oracle Publishers](../../relational-databases/replication/non-sql/troubleshooting-oracle-publishers.md).|Les autorisations associées à la connexion administrateur du serveur de publication Oracle '% s' ne suffisent pas.|  
|[MSSQL_ENG021797](../../relational-databases/replication/mssql-eng021797.md)|'%s' doit être une connexion Windows valide sous la forme 'MACHINE\Login' ou 'DOMAIN\Login'. Consultez la documentation de '%s'.|  
|[MSSQL_ENG021798](../../relational-databases/replication/mssql-eng021798.md)|Le travail de l'Agent '%s' doit être ajouté à l'aide de '%s' avant de continuer. Consultez la documentation de '%s'.|  
|[MSSQL_REPL020011](../../relational-databases/replication/mssql-repl020011.md)|Le processus n'a pas pu exécuter '%1' sur '%2'.|  
|[MSSQL_REPL027056](../../relational-databases/replication/mssql-repl027056.md)|Le processus de fusion n'a pas pu modifier l'historique de génération sur le '%1'. Lors de la résolution du problème, redémarrez la synchronisation avec un enregistrement d'historique détaillé et spécifiez un fichier de sortie dans lequel écrire.|  
|[MSSQL_REPL027183](../../relational-databases/replication/mssql-repl027183.md)|Le processus de fusion n'a pas pu énumérer les modifications apportées aux articles via le filtre de lignes paramétrable. Si le problème persiste, augmentez le délai de requête du processus, réduisez la période de rétention de la publication, puis améliorez les index des tables publiées.|  
  
  
