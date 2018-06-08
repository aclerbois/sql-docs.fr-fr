---
title: Connexion à Sybase ASE (SybaseToSQL) | Documents Microsoft
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.component: ssma-sybase
ms.reviewer: ''
ms.suite: sql
ms.technology: ssma
ms.tgt_pltfrm: ''
ms.topic: conceptual
applies_to:
- Azure SQL Database
- SQL Server
helpviewer_keywords:
- Connecting to Sybase ASE
ms.assetid: a45a2330-9175-4c9e-af38-ef920e350614
caps.latest.revision: 8
author: Shamikg
ms.author: Shamikg
manager: craigg
ms.openlocfilehash: ac144d636536bac66f7330825f536f01200738b6
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="connecting-to-sybase-ase-sybasetosql"></a>Connexion à Sybase ASE (SybaseToSQL)
Pour migrer des bases de données Sybase Adaptive Server Enterprise (ASE) [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] ou SQL Azure, vous devez vous connecter au serveur adaptatif qui contient les bases de données que vous souhaitez migrer. Lorsque vous vous connectez, SSMA Obtient les métadonnées relatives à toutes les bases de données sur le serveur Adaptive et affiche les métadonnées de la base de données dans le volet Explorateur de métadonnées de Sybase. SSMA stocke des informations sur le serveur de base de données, mais ne stocke pas les mots de passe.  
  
Votre connexion à ASE reste active jusqu'à ce que vous fermez le projet. Lorsque vous rouvrez le projet, vous devez vous reconnecter à ASE si vous souhaitez une connexion active au serveur.  
  
Métadonnées relatives à Adaptive Server ne sont pas automatiquement mis à jour. Au lieu de cela, si vous souhaitez mettre à jour les métadonnées dans l’Explorateur de métadonnées Sybase, vous devez manuellement mettre à jour les métadonnées, comme décrit dans la section « Actualisation des métadonnées Sybase ASE » plus loin dans cette rubrique.  
  
## <a name="required-ase-permissions"></a>Autorisations requises ASE  
Le compte qui est utilisé pour se connecter à ASE doit avoir au moins **public** accès à la base de données master et aux bases de données source à migrer vers [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] ou SQL Azure. En outre, pour sélectionner les autorisations sur les tables qui sont en cours de migration, l’utilisateur doit disposer des autorisations SELECT sur les tables système suivantes :  
  
-   [source_db].dbo.sysobjects  
  
-   [source_db].dbo.syscolumns  
  
-   [source_db].dbo.sysusers  
  
-   [source_db].dbo.systypes  
  
-   [source_db].dbo.sysconstraints  
  
-   [source_db].dbo.syscomments  
  
-   [source_db].dbo.sysindexes  
  
-   [source_db].dbo.sysreferences  
  
-   master.dbo.sysdatabases  
  
## <a name="establishing-a-connection-to-ase"></a>L’établissement d’une connexion à ASE  
Lorsque vous vous connectez à un serveur Adaptive, SSMA lit les métadonnées de la base de données sur le serveur de base de données, puis ajoute ces métadonnées dans le fichier projet. Ces métadonnées sont utilisées par SSMA lorsqu’il convertit les objets à [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] ou la syntaxe SQL Azure, et lorsqu’il migre les données pour [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] ou SQL Azure. Vous pouvez parcourir ces métadonnées dans le volet Explorateur de métadonnées Sybase et passez en revue les propriétés des objets de base de données individuels.  
  
> [!IMPORTANT]  
> Avant d’essayer de se connecter au serveur de base de données, assurez-vous que le serveur de base de données est en cours d’exécution et qu’il peut accepter des connexions.  
  
**Pour vous connecter à Sybase ASE**  
  
1.  Sur le **fichier** menu, sélectionnez **se connecter à Sybase**.  
  
    Si vous avez connecté précédemment pour Sybase, le nom de la commande sera **se reconnecter pour Sybase**.  
  
2.  Dans le **fournisseur** , sélectionnez un des fournisseurs installés sur l’ordinateur de se connecter au serveur de Sybase.  
  
3.  Dans le **Mode** , sélectionnez soit **mode Standard** ou **mode avancé**.  
  
    Utiliser le mode standard pour spécifier le nom du serveur, le port, le nom d’utilisateur et le mot de passe. Utilisez le mode avancé pour fournir une chaîne de connexion. Ce mode est généralement utilisé uniquement pour le dépannage ou l’utilisation de support technique.  
  
4.  Si vous sélectionnez **mode Standard**, indiquez les valeurs suivantes :  
  
    1.  Dans le **nom du serveur** zone, entrez ou sélectionnez le nom ou l’adresse IP du serveur de base de données.  
  
    2.  Si le serveur de base de données n’est pas configuré pour accepter les connexions sur la valeur par défaut (5000) du port, entrez le numéro de port qui est utilisé pour les connexions de Sybase dans le **port du serveur** boîte.  
  
    3.  Dans le **nom d’utilisateur** , entrez un compte de Sybase qui dispose des autorisations nécessaires.  
  
    4.  Dans le **mot de passe** , entrez le mot de passe pour le nom d’utilisateur spécifié.  
  
5.  Si vous sélectionnez **mode avancé**, fournir une chaîne de connexion dans le **chaîne de connexion** boîte.  
  
    Exemples de chaînes de connexion différentes sont les suivantes :  
  
    1.  **Chaînes de connexion pour le fournisseur OLE DB Sybase :**  
  
        Pour Sybase ASE OLE DB 12,5, un exemple de chaîne de connexion est la suivante :  
  
        `Server Name=sybserver;User ID=MyUserID;Password=MyP@$$word;Provider=Sybase.ASEOLEDBProvider;`  
  
        Pour Sybase ASE OLE DB 15, un exemple de chaîne de connexion est la suivante :  
  
        `Server=sybserver;User ID=MyUserID;Password=MyP@$$word;Provider= ASEOLEDB;Port=5000;`  
  
    2.  **Chaîne de connexion pour le fournisseur de ODBC Sybase :**  
  
        `Driver=Adaptive Server Enterprise;Server=sybserver;uid=MyUserID;pwd=MyP@$$word;Port=5000;`  
  
    3.  **Chaîne de connexion pour le fournisseur ADO.NET de Sybase :**  
  
        `Server=sybserver;Port=5000;uid=MyUserID;pwd=MyP@$$word;`  
  
    Pour plus d’informations, consultez [se connecter à Sybase &#40;SybaseToSQL&#41;](../../ssma/sybase/connect-to-sybase-sybasetosql.md).  
  
## <a name="reconnecting-to-sybase-ase"></a>Rétablir la connexion à Sybase ASE  
Votre connexion au serveur de base de données reste active jusqu'à ce que vous fermez le projet. Lorsque vous rouvrez le projet, vous devez vous reconnecter si vous souhaitez une connexion active à Adaptive Server. Vous pouvez travailler hors connexion jusqu'à ce que vous souhaitez mettre à jour les métadonnées, charger des objets de base de données dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] ou SQL Azure, et migrer les données.  
  
## <a name="refreshing-sybase-ase-metadata"></a>L’actualisation des métadonnées de Sybase ASE  
Les métadonnées sur les bases de données ASE ne sont pas actualisée automatiquement. Les métadonnées dans l’Explorateur de métadonnées Sybase sont un instantané des métadonnées lorsque vous tout d’abord connecté au serveur adaptative ou de la dernière fois que vous actualisées manuellement les métadonnées. Vous pouvez mettre à jour manuellement les métadonnées pour une base de données, un schéma de base de données unique ou toutes les bases de données.  
  
**Pour actualiser les métadonnées**  
  
1.  Assurez-vous que vous êtes connecté au serveur adaptative.  
  
2.  Dans l’Explorateur de métadonnées de Sybase, sélectionnez la case à cocher en regard de la base de données ou le schéma de base de données que vous souhaitez mettre à jour.  
  
3.  Cliquez sur les bases de données ou base de données individuelle ou schéma de base de données, puis sélectionnez **Actualiser à partir de la base de données**.  
  
4.  Si vous êtes invité à vérifier l’objet en cours, cliquez sur **Oui**.  
  
## <a name="next-step"></a>Étape suivante  
  
-   L’étape suivante du processus de migration consiste à [se connecter à une instance de SQL Server](http://msdn.microsoft.com/en-us/dd368a1a-45b0-40e9-b4d3-5cdb48c26606) / [connexion à une instance de SQL Azure](http://msdn.microsoft.com/en-us/9e77e4b0-40c0-455c-8431-ca5d43849aa7)  
  
## <a name="see-also"></a>Voir aussi  
[Migration des bases de données de Sybase ASE à SQL Server - base de données SQL Azure &#40;SybaseToSQL&#41;](../../ssma/sybase/migrating-sybase-ase-databases-to-sql-server-azure-sql-db-sybasetosql.md)  
  
