---
title: Connexion à la base de données DB2 (DB2ToSQL) | Documents Microsoft
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
ms.assetid: 5eb5801d-f0c3-4127-97c0-0b1ef49f4844
caps.latest.revision: 7
author: Shamikg
ms.author: Shamikg
manager: craigg
ms.openlocfilehash: d9f3d4b687c86804cc2339d675333c3a7e73ff6b
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="connecting-to-db2-database-db2tosql"></a>Connexion à la base de données DB2 (DB2ToSQL)
Pour migrer des bases de données DB2 [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)], vous devez vous connecter à la base de données DB2 que vous souhaitez migrer. Lorsque vous vous connectez, SSMA Obtient les métadonnées relatives à tous les schémas de DB2, puis l’affiche dans le volet Explorateur de métadonnées de DB2. SSMA stocke des informations sur le serveur de base de données, mais ne stocke pas les mots de passe.  
  
Votre connexion à la base de données reste active jusqu'à ce que vous fermez le projet. Lorsque vous rouvrez le projet, vous devez vous reconnecter si vous souhaitez une connexion active à la base de données.  
  
Métadonnées relatives à la base de données DB2 ne sont pas automatiquement mis à jour. Au lieu de cela, si vous souhaitez mettre à jour les métadonnées dans l’Explorateur de métadonnées DB2, vous devez manuellement mettre à jour. Pour plus d’informations, consultez la section « Actualisation des métadonnées de DB2 » plus loin dans cette rubrique.  
  
## <a name="required-db2-permissions"></a>Autorisations de DB2 requis  
Autorisation de l’utilisateur définit la liste des commandes et des objets qui sont disponibles pour un utilisateur. Cette liste de contrôle ainsi les actions de l’utilisateur. Dans DB2, il existe des groupes prédéfinis de privilèges pour l’autorisation au niveau de l’instance et au niveau d’une base de données DB2. Cela permet de SSMA obtenir les métadonnées à partir de schémas appartenant à l’utilisateur connecté. Pour obtenir les métadonnées pour les objets dans d’autres schémas, puis la convertir les objets dans ces schémas, le compte doit disposer des autorisations suivantes :  
  
-   Schéma pour la migration de schéma normalement l’accès public, sauf si le mot clé RESTRICT a été utilisé dans la création  
  
-   Accès aux données pour la migration des données requiert l’accès aux données  
  
## <a name="establishing-a-connection-to-db2"></a>L’établissement d’une connexion à DB2  
Lorsque vous vous connectez à une base de données, SSMA lit les métadonnées de la base de données, puis ajoute ces métadonnées dans le fichier projet. Ces métadonnées sont utilisées par SSMA lorsqu’il convertit des objets à [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] syntaxe, et quand il migre les données à [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)]. Vous pouvez parcourir ces métadonnées dans le volet Explorateur de métadonnées DB2 et passez en revue les propriétés des objets de base de données individuels.  
  
> [!IMPORTANT]  
> Avant d’essayer de se connecter, vérifiez que le serveur de base de données est en cours d’exécution et peut accepter des connexions.  
  
**Pour vous connecter à DB2**  
  
1.  Sur le **fichier** menu, sélectionnez **se connecter à DB2**.  
  
    Si vous déjà connecté à DB2, le nom de la commande sera **se reconnecter à DB2**.  
  
2.  Dans le **fournisseur** zone, vous verrez la **fournisseur OLE DB** qui est actuellement le fournisseur d’accès client DB2 uniquement.  
  
3.  Dans le **Manager** zone, vous pouvez sélectionner **Db2 pour zOs**, ou **DB2 pour LUW**  
  
4.  Dans le **Mode** , sélectionnez soit **mode Standard**, ou **mode de chaîne de connexion**.  
  
    Utiliser le mode standard pour spécifier le nom du serveur et le port. Utilisez le mode de nom de service pour spécifier le nom du service DB2 manuellement. Utilisez le mode de chaîne de connexion pour fournir une chaîne de connexion complète.  
  
5.  Si vous sélectionnez **mode Standard**, indiquez les valeurs suivantes :  
  
    -   Dans le **nom du serveur** zone, entrez ou sélectionnez le nom ou l’adresse IP du serveur de base de données.  
  
    -   Si le serveur de base de données n’est pas configuré pour accepter les connexions sur la valeur par défaut (1521) du port, entrez le numéro de port qui est utilisé pour les connexions DB2 dans le **port du serveur** boîte.  
  
    -   Dans le **Port du serveur** , entrez le numéro de Port TCP/IP.  
  
    -   Dans le **Initial Catalog** , entrez le nom de la base de données  
  
    -   Dans le **nom d’utilisateur** , entrez un compte de DB2 qui dispose des autorisations nécessaires.  
  
    -   Dans le **mot de passe** , entrez le mot de passe pour le nom d’utilisateur spécifié.  
  
6.  Si vous sélectionnez **mode de chaîne de connexion**, fournir une chaîne de connexion dans le **chaîne de connexion** boîte.  
  
    L’exemple suivant montre une chaîne de connexion OLE DB :  
  
    `Provider=OraOLEDB.DB2;Data Source=MyDB2DB;User Id=myUsername;Password=myPassword;`  
  
    L’exemple suivant montre une chaîne de connexion DB2 Client qui utilise la sécurité intégrée :  
  
    `Data Source=MyDB2DB;Integrated Security=yes;`  
  
    Pour plus d’informations, consultez [se connecter à Oracle &#40;OracleToSQL&#41;](../../ssma/oracle/connect-to-oracle-oracletosql.md).  
  
## <a name="reconnecting-to-db2"></a>Rétablir la connexion à DB2  
Votre connexion au serveur de base de données reste active jusqu'à ce que vous fermez le projet. Lorsque vous rouvrez le projet, vous devez vous reconnecter si vous souhaitez une connexion active à la base de données. Vous pouvez travailler hors connexion jusqu'à ce que vous souhaitez mettre à jour les métadonnées, charger des objets de base de données dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)], et migrer les données.  
  
## <a name="refreshing-db2-metadata"></a>Actualisation des métadonnées de DB2  
Métadonnées relatives à la base de données DB2 ne sont pas actualisée automatiquement. Les métadonnées dans l’Explorateur de métadonnées DB2 sont un instantané de métadonnées lors de la première connexion, ou la dernière fois que vous actualisées manuellement les métadonnées. Vous pouvez mettre à jour manuellement les métadonnées pour tous les schémas, un seul schéma ou des objets de base de données individuels.  
  
**Pour actualiser les métadonnées**  
  
1.  Assurez-vous que vous êtes connecté à la base de données.  
  
2.  Dans l’Explorateur de métadonnées de DB2, sélectionnez la case à cocher en regard de chaque objet de schéma ou de base de données que vous souhaitez mettre à jour.  
  
3.  Avec le bouton droit **schémas**, ou le schéma individuel ou la base de données de l’objet, puis sélectionnez **Actualiser à partir de la base de données**.  
  
    Si vous n’avez pas d’une connexion active, SSMA affichera le **se connecter à DB2** boîte de dialogue afin de pouvoir vous connecter.  
  
4.  Dans l’actualisation à partir de la boîte de dialogue base de données, spécifiez les objets à actualiser.  
  
    -   Pour actualiser un objet, cliquez sur le **Active** champ adjacent à l’objet jusqu'à ce qu’une flèche s’affiche.  
  
    -   Pour empêcher qu’un objet en cours d’actualisation, cliquez sur le **Active** champ adjacente à l’objet jusqu'à ce qu’une **X** s’affiche.  
  
    -   Pour actualiser ou refuser une catégorie d’objets, cliquez sur le **Active** champ adjacent au dossier de catégorie.  
  
    Pour afficher les définitions du codage en couleurs, cliquez sur le **légende** bouton.  
  
5.  [!INCLUDE[clickOK](../../includes/clickok_md.md)]  
  
## <a name="next-step"></a>Étape suivante  
  
-   L’étape suivante du processus de migration consiste à [connexion à SQL Server](http://msdn.microsoft.com/en-us/b59803cb-3cc6-41cc-8553-faf90851410e).  
  
## <a name="see-also"></a>Voir aussi  
[Bases de données DB2 migration vers SQL Server &#40;DB2ToSQL&#41;](../../ssma/db2/migrating-db2-databases-to-sql-server-db2tosql.md)  
  
