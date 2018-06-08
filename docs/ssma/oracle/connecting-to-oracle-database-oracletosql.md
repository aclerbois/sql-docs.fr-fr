---
title: Connexion à la base de données Oracle (OracleToSQL) | Documents Microsoft
ms.prod: sql
ms.prod_service: sql-tools
ms.component: ssma-oracle
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.suite: sql
ms.technology: ssma
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- Refreshing Oracle Metadata
ms.assetid: e276cdbf-3ebc-4ba8-b40d-a7a42befa2b6
caps.latest.revision: 9
author: Shamikg
ms.author: Shamikg
manager: v-thobro
ms.openlocfilehash: 0673afd74ef1c11b9c800d128ea25e0189a635e9
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="connecting-to-oracle-database-oracletosql"></a>Connexion à la base de données Oracle (OracleToSQL)
Pour migrer des bases de données Oracle [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)], vous devez vous connecter à la base de données Oracle que vous souhaitez migrer. Lorsque vous vous connectez, SSMA Obtient les métadonnées relatives à tous les schémas Oracle, puis l’affiche dans le volet Explorateur de métadonnées d’Oracle. SSMA stocke des informations sur le serveur de base de données, mais ne stocke pas les mots de passe.  
  
Votre connexion à la base de données reste active jusqu'à ce que vous fermez le projet. Lorsque vous rouvrez le projet, vous devez vous reconnecter si vous souhaitez une connexion active à la base de données.  
  
Métadonnées relatives à la base de données Oracle ne sont pas automatiquement mis à jour. Au lieu de cela, si vous souhaitez mettre à jour les métadonnées dans l’Explorateur de métadonnées Oracle, vous devez manuellement mettre à jour. Pour plus d’informations, consultez la section « Actualisation des métadonnées Oracle » plus loin dans cette rubrique.  
  
## <a name="required-oracle-permissions"></a>Autorisations requises Oracle  
Le compte qui est utilisé pour se connecter à la base de données Oracle doit disposer au moins **CONNECT** autorisations. Cela permet de SSMA obtenir les métadonnées à partir de schémas appartenant à l’utilisateur connecté. Pour obtenir les métadonnées pour les objets dans d’autres schémas, puis la convertir les objets dans ces schémas, le compte doit disposer des autorisations suivantes :  
  
-   CRÉER UNE PROCÉDURE  
  
-   EXÉCUTER UNE PROCÉDURE  
  
-   SÉLECTIONNEZ N’IMPORTE QUELLE TABLE  
  
-   SÉLECTIONNEZ N’IMPORTE QUELLE SÉQUENCE  
  
-   CRÉER N’IMPORTE QUEL TYPE  
  
-   CRÉER UN DÉCLENCHEUR  
  
-   SÉLECTIONNER UN DICTIONNAIRE  
  
## <a name="establishing-a-connection-to-oracle"></a>L’établissement d’une connexion à Oracle  
Lorsque vous vous connectez à une base de données, SSMA lit les métadonnées de la base de données, puis ajoute ces métadonnées dans le fichier projet. Ces métadonnées sont utilisées par SSMA lorsqu’il convertit des objets à [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] syntaxe, et quand il migre les données à [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)]. Vous pouvez parcourir ces métadonnées dans le volet Explorateur de métadonnées d’Oracle et passez en revue les propriétés des objets de base de données individuels.  
  
> [!IMPORTANT]  
> Avant d’essayer de se connecter, vérifiez que le serveur de base de données est en cours d’exécution et peut accepter des connexions.  
  
**Pour vous connecter à Oracle**  
  
1.  Sur le **fichier** menu, sélectionnez **se connecter à Oracle**.  
  
    Si vous déjà connecté à Oracle, le nom de la commande sera **se reconnecter à Oracle**.  
  
2.  Dans le **fournisseur** boîte, sélectionnez **fournisseur Client Oracle** ou **fournisseur OLE DB**, selon le fournisseur est installé. La valeur par défaut est le client Oracle.  
  
3.  Dans le **Mode** , sélectionnez soit **mode Standard**, **le mode TNSNAME**, ou **mode de chaîne de connexion**.  
  
    Utiliser le mode standard pour spécifier le nom du serveur et le port. Utilisez le mode de nom de service pour spécifier le nom du service Oracle manuellement. Utilisez le mode de chaîne de connexion pour fournir une chaîne de connexion complète.  
  
4.  Si vous sélectionnez **mode Standard**, indiquez les valeurs suivantes :  
  
    1.  Dans le **nom du serveur** zone, entrez ou sélectionnez le nom ou l’adresse IP du serveur de base de données.  
  
    2.  Si le serveur de base de données n’est pas configuré pour accepter les connexions sur la valeur par défaut (1521) du port, entrez le numéro de port qui est utilisé pour les connexions Oracle dans la **port du serveur** boîte.  
  
    3.  Dans le **SID Oracle** , entrez l’identificateur du système.  
  
    4.  Dans le **nom d’utilisateur** , entrez un compte d’Oracle qui dispose des autorisations nécessaires.  
  
    5.  Dans le **mot de passe** , entrez le mot de passe pour le nom d’utilisateur spécifié.  
  
5.  Si vous sélectionnez **le mode TNSNAME**, indiquez les valeurs suivantes :  
  
    1.  Dans le **identificateur de connexion** , entrez l’identificateur (alias TNS) de la base de données de connexion.  
  
    2.  Dans le **nom d’utilisateur** , entrez un compte d’Oracle qui dispose des autorisations nécessaires.  
  
    3.  Dans le **mot de passe** , entrez le mot de passe pour le nom d’utilisateur spécifié.  
  
6.  Si vous sélectionnez **mode de chaîne de connexion**, fournir une chaîne de connexion dans le **chaîne de connexion** boîte.  
  
    L’exemple suivant montre une chaîne de connexion OLE DB :  
  
    `Provider=OraOLEDB.Oracle;Data Source=MyOracleDB;User Id=myUsername;Password=myPassword;`  
  
    L’exemple suivant montre une chaîne de connexion du Client Oracle qui utilise la sécurité intégrée :  
  
    `Data Source=MyOracleDB;Integrated Security=yes;`  
  
    Pour plus d’informations, consultez [se connecter à Oracle &#40;OracleToSQL&#41;](../../ssma/oracle/connect-to-oracle-oracletosql.md).  
  
## <a name="reconnecting-to-oracle"></a>Rétablir la connexion à Oracle  
Votre connexion au serveur de base de données reste active jusqu'à ce que vous fermez le projet. Lorsque vous rouvrez le projet, vous devez vous reconnecter si vous souhaitez une connexion active à la base de données. Vous pouvez travailler hors connexion jusqu'à ce que vous souhaitez mettre à jour les métadonnées, charger des objets de base de données dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)], et migrer les données.  
  
## <a name="refreshing-oracle-metadata"></a>Actualisation des métadonnées d’Oracle  
Métadonnées relatives à la base de données Oracle ne sont pas actualisée automatiquement. Les métadonnées dans l’Explorateur de métadonnées Oracle sont un instantané de métadonnées lors de la première connexion, ou la dernière fois que vous actualisées manuellement les métadonnées. Vous pouvez mettre à jour manuellement les métadonnées pour tous les schémas, un seul schéma ou des objets de base de données individuels.  
  
**Pour actualiser les métadonnées**  
  
1.  Assurez-vous que vous êtes connecté à la base de données.  
  
2.  Dans l’Explorateur de métadonnées d’Oracle, sélectionnez la case à cocher en regard de chaque objet de schéma ou de base de données que vous souhaitez mettre à jour.  
  
3.  Avec le bouton droit **schémas**, ou le schéma individuel ou la base de données de l’objet, puis sélectionnez **Actualiser à partir de la base de données**.  
  
    Si vous n’avez pas d’une connexion active, SSMA affichera le **se connecter à Oracle** boîte de dialogue afin de pouvoir vous connecter.  
  
4.  Dans l’actualisation à partir de la boîte de dialogue base de données, spécifiez les objets à actualiser.  
  
    -   Pour actualiser un objet, cliquez sur le **Active** champ adjacent à l’objet jusqu'à ce qu’une flèche s’affiche.  
  
    -   Pour empêcher qu’un objet en cours d’actualisation, cliquez sur le **Active** champ adjacente à l’objet jusqu'à ce qu’une **X** s’affiche.  
  
    -   Pour actualiser ou refuser une catégorie d’objets, cliquez sur le **Active** champ adjacent au dossier de catégorie.  
  
    Pour afficher les définitions du codage en couleurs, cliquez sur le **légende** bouton.  
  
5.  [!INCLUDE[clickOK](../../includes/clickok_md.md)]  
  
## <a name="next-step"></a>Étape suivante  
  
-   L’étape suivante du processus de migration consiste à [se connecter à une instance de SQL Server](http://msdn.microsoft.com/en-us/1b2a8059-1829-4904-a82f-9c06de1e245f).  
  
## <a name="see-also"></a>Voir aussi  
[Bases de données de migration d’Oracle vers SQL Server &#40;OracleToSQL&#41;](../../ssma/oracle/migrating-oracle-databases-to-sql-server-oracletosql.md)  
  
