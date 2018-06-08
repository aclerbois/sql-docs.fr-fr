---
title: Connexion à SQL Server (DB2eToSQL) | Documents Microsoft
ms.prod: sql
ms.prod_service: sql-tools
ms.component: ssma-db2
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.suite: sql
ms.technology: ssma
ms.tgt_pltfrm: ''
ms.topic: conceptual
applies_to:
- Azure SQL Database
- SQL Server
ms.assetid: b59803cb-3cc6-41cc-8553-faf90851410e
caps.latest.revision: 8
author: Shamikg
ms.author: Shamikg
manager: craigg
ms.openlocfilehash: d257c6b2569eec1677577e8fe98a0fe619ab60f1
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="connecting-to-sql-server-db2etosql"></a>Connexion à SQL Server (DB2eToSQL)
Pour migrer des bases de données DB2 [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] 2012, [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)]2014 ou Azure SQL DB vous devez vous connecter à un de ces instances de la cible de [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)]. Lorsque vous vous connectez, SSMA Obtient les métadonnées relatives à toutes les bases de données dans l’instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] et affiche les métadonnées de la base de données dans le [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] Explorateur de métadonnées. SSMA stocke des informations sur l’instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] vous êtes connecté à, mais ne stocke pas les mots de passe.  
  
Votre connexion à [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] reste active jusqu'à ce que vous fermez le projet. Lorsque vous rouvrez le projet, vous devez vous reconnecter à [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] si vous souhaitez une connexion active au serveur. Vous pouvez travailler hors connexion jusqu'à ce que vous chargez des objets de base de données dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] et migrer les données.  
  
Métadonnées relatives à l’instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] n’est pas synchronisé automatiquement. Au lieu de cela, pour mettre à jour les métadonnées dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] l’Explorateur de métadonnées, vous devez manuellement mettre à jour le [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] métadonnées. Pour plus d’informations, consultez le « Synchronizing [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] métadonnées » plus loin dans cette rubrique.  
  
## <a name="required-sql-server-permissions"></a>Autorisations requises de SQL Server  
Le compte qui est utilisé pour se connecter à [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] nécessite des autorisations différentes selon les actions réalisées par le compte :  
  
-   Pour convertir des objets DB2 à [!INCLUDE[tsql](../../includes/tsql_md.md)] syntaxe, pour mettre à jour des métadonnées à partir de [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)], ou pour enregistrer la syntaxe converti aux scripts, le compte doit disposer d’autorisation de se connecter à l’instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)].  
  
-   Pour charger des objets de base de données dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)], le compte doit être un membre de la **sysadmin** rôle de serveur. Cela est nécessaire pour installer les assemblys CLR.  
  
-   Pour migrer des données vers [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)], le compte doit être un membre de la **sysadmin** rôle de serveur. Cela est nécessaire pour exécuter le [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] packages de migration de données de l’Agent.  
  
-   Pour exécuter le code qui est généré par SSMA, le compte doit avoir **Execute** autorisations pour toutes les fonctions définies par l’utilisateur dans le **ssma_DB2** schéma de la base de données cible. Ces fonctions fournissent des fonctionnalités équivalentes des fonctions du système DB2 et sont utilisées par les objets convertis.  
  
Si le compte qui est utilisé pour se connecter à [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] est pour effectuer la migration de toutes les tâches, le compte doit être un membre de la **sysadmin** rôle de serveur.  
  
## <a name="establishing-a-sql-server-connection"></a>L’établissement d’une connexion SQL Server  
Avant de convertir les objets de base de données DB2 à [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] syntaxe, vous devez établir une connexion à l’instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] où vous souhaitez migrer les bases de données ou la base de données DB2.  
  
Lorsque vous définissez les propriétés de connexion, vous spécifiez également la base de données où les objets et les données seront migrées. Vous pouvez personnaliser ce mappage au niveau du schéma de DB2 après vous être connecté à [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)]. Pour plus d’informations, consultez [mappage des schémas de DB2 à des schémas SQL Server &#40;DB2ToSQL&#41;](../../ssma/db2/mapping-db2-schemas-to-sql-server-schemas-db2tosql.md).  
  
> [!IMPORTANT]  
> Avant d’essayer de se connecter à [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)], assurez-vous que l’instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] est en cours d’exécution et peut accepter des connexions.  
  
**Pour se connecter à SQL Server**  
  
1.  Sur le **fichier** menu, sélectionnez **se connecter à SQL Server**.  
  
    Si vous aviez précédemment connecté à [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)], le nom de la commande sera **se reconnecter à SQL Server**.  
  
2.  Dans la boîte de dialogue de connexion, entrez ou sélectionnez le nom de l’instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)].  
  
    -   Si vous vous connectez à l’instance par défaut sur l’ordinateur local, vous pouvez entrer **localhost** ou un point (**.**).  
  
    -   Si vous vous connectez à l’instance par défaut sur un autre ordinateur, entrez le nom de l’ordinateur.  
  
    -   Si vous vous connectez à une instance nommée sur un autre ordinateur, entrez le nom d’ordinateur suivi d’une barre oblique inverse, puis le nom d’instance, tels que MyServer\MyInstance.  
  
3.  Si votre instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] est configuré pour accepter les connexions sur un port non défini par défaut, entrez le numéro de port qui est utilisé pour [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] connexions dans le **port du serveur** boîte. L’instance par défaut de [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)], le numéro de port par défaut est 1433. Pour les instances nommées, SSMA essaiera d’obtenir le numéro de port à partir de la [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] Service Browser.  
  
4.  Dans le **base de données** , entrez le nom de la base de données cible.  
  
    Cette option n’est pas disponible lorsque vous vous reconnectez à [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)].  
  
5.  Dans le **authentification** , sélectionnez le type d’authentification à utiliser pour la connexion. Pour utiliser le compte Windows actuel, sélectionnez **l’authentification Windows**. Pour utiliser un [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] connexion, sélectionnez **l’authentification SQL Server**, puis fournissez le nom de connexion et un mot de passe.  
  
6.  Pour une connexion sécurisée, les deux contrôles sont ajoutés, le **chiffrer la connexion** et **TrustServerCertificate** cases à cocher. Uniquement lorsque **chiffrer la connexion** est activée, le **TrustServerCertificate** case à cocher est visible. Lorsque **chiffrer la connexion** est vérifiée (true) et **TrustServerCertificate** est désactivée (false), il valide le certificat SSL SQL Server. La validation du certificat de serveur est une partie de la négociation SSL qui garantit qu'il s'agit du serveur correct avec lequel établir une connexion. Pour ce faire, un certificat doit être installé sur le côté client, ainsi que sur le côté serveur.  
  
7.  Cliquez sur **Se connecter**.  
  
**Compatibilité de Version supérieure**  
  
-   Vous serez en mesure de se connecter à [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] 2008 et [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] 2012 et [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] 2014 et [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] 2016 lorsque le projet de migration créé est [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] 2005.  
  
-   Vous serez en mesure de se connecter à [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] 2012 et [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] 2014 et [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] 2016 lorsque le projet de migration créé est [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] 2008, mais vous ne pourrez pas connecter aux versions inférieures c'est-à-dire [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] 2005.  
  
-   Vous serez en mesure de se connecter à [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] 2012 et [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] 2014 et [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] 2016 lorsque le projet créé est SQL Server 2012.  
  
||||||  
|-|-|-|-|-|  
|**TYPE et VERSION du serveur cible du projet**|[!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] 2012 <br />(Version:11.x)|[!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] 2014 <br />(Version:12.x)|[!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] 2016 <br />(Version:13.x)|Base de données SQL Azure|  
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] 2012|Oui|Oui|Oui||  
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] 2014||Oui|Oui||  
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] 2014|||Oui||  
|Base de données SQL Azure||||Oui|  
  
> [!IMPORTANT]  
> Conversion des objets de base de données est effectuée selon le type de projet, mais pas conformément à la version de la [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] vous êtes connecté. Dans le cas de [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] 2012, [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] 2014, [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] base de données SQL 2016 ou Azure.  
  
## <a name="synchronizing-sql-server-metadata"></a>Synchronisation des métadonnées du serveur SQL  
Métadonnées relatives à [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] bases de données n’est pas automatiquement mis à jour. Les métadonnées dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] Explorateur de métadonnées est un instantané des métadonnées lorsque vous avez connecté tout d’abord à [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)], ou la dernière fois que vous les métadonnées mises à jour manuellement. Vous pouvez mettre à jour manuellement les métadonnées pour toutes les bases de données, ou pour tout de la base de données unique ou d’un objet de base de données.  
  
**Pour synchroniser les métadonnées**  
  
1.  Assurez-vous que vous êtes connecté à [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)].  
  
2.  Dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] Explorateur de métadonnées, activez la case à cocher en regard de la base de données ou le schéma que vous souhaitez mettre à jour la base de données.  
  
    Par exemple, pour mettre à jour les métadonnées de toutes les bases de données, activez la case à côté **bases de données**.  
  
3.  Avec le bouton droit **bases de données**, ou le base de données individuelle ou schéma de base de données, puis **synchroniser avec la base de données**.  
  
## <a name="next-step"></a>Étape suivante  
L’étape suivante de la migration dépend des besoins de votre projet :  
  
-   Pour personnaliser le mappage entre les schémas de DB2 et [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] bases de données et des schémas, consultez [de schémas SQL Server, les schémas de mappage DB2 &#40;DB2ToSQL&#41;](../../ssma/db2/mapping-db2-schemas-to-sql-server-schemas-db2tosql.md).  
  
-   Pour personnaliser les options de configuration pour les projets, consultez [les paramètres de projet &#40;Conversion&#41; &#40;DB2ToSQL&#41; ](../../ssma/db2/project-settings-conversion-db2tosql.md) et rubriques connexes.  
  
-   Pour personnaliser le mappage des types de données source et cible, consultez [DB2 de mappage et les Types de données SQL Server &#40;DB2ToSQL&#41;](../../ssma/db2/mapping-db2-and-sql-server-data-types-db2tosql.md).  
  
-   Si vous n’avez pas à effectuer ces tâches, vous pouvez convertir les définitions d’objets de base de données DB2 dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] définitions d’objets. Pour plus d’informations, consultez [conversion de schémas de DB2 &#40;DB2ToSQL&#41;](../../ssma/db2/converting-db2-schemas-db2tosql.md).  
  
## <a name="see-also"></a>Voir aussi  
[Bases de données DB2 migration vers SQL Server &#40;DB2ToSQL&#41;](../../ssma/db2/migrating-db2-databases-to-sql-server-db2tosql.md)  
  
