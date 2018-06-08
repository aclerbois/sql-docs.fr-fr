---
title: Migration de données DB2 dans SQL Server (DB2ToSQL) | Documents Microsoft
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
ms.assetid: 86cbd39f-6dac-409a-9ce1-7dd54403f84b
caps.latest.revision: 5
author: Shamikg
ms.author: Shamikg
manager: craigg
ms.openlocfilehash: ffcbf3b0e59026fea0005cd3d955de6b4af89059
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="migrating-db2-data-into-sql-server-db2tosql"></a>Migration de données DB2 dans SQL Server (DB2ToSQL)
Après avoir synchronisé correctement les objets convertis avec [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)], vous pouvez migrer des données à partir de DB2 pour [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)].  
  
> [!IMPORTANT]  
> Si le moteur utilisé est le moteur de Migration de données côté serveur, puis, avant de pouvoir migrer les données, vous devez installer SSMA pour le Pack d’Extension de DB2 et les fournisseurs de DB2 sur l’ordinateur qui est en cours d’exécution SSMA. Le service de l’Agent SQL Server doit également être en cours d’exécution. Pour plus d’informations sur la façon d’installer le pack d’extension, consultez [l’installation des composants de SSMA sur SQL Server](http://msdn.microsoft.com/en-us/cf2b724b-4ca7-470a-8dd7-fa95b1e060a4)  
  
## <a name="setting-migration-options"></a>Définition des Options de Migration  
Avant de migrer les données pour [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)], passez en revue les options de migration de projet dans le **les paramètres de projet** boîte de dialogue.  
  
-   À l’aide de cette boîte de dialogue, vous pouvez définir les options telles que la taille de lot de migration, verrouillage de table, la vérification des contraintes, la gestion des valeurs null et la gestion de valeur d’identité. Pour plus d’informations sur les paramètres de Migration de projet, consultez [paramètres du projet (Migration)](http://msdn.microsoft.com/en-us/48aaa8e6-a9cb-487d-9ba5-fc3f1c4786ae).  
  
-   Le **moteur de Migration** dans les **les paramètres de projet** boîte de dialogue permet à l’utilisateur effectuer le processus de migration à l’aide de deux types de moteurs de migration de données :  
  
    1.  Moteur de Migration de données côté client  
  
    2.  Moteur de Migration de données côté serveur  
  
**Migration des données côté client :**  
  
-   Pour lancer la migration des données côté client, sélectionnez le **moteur de Migration de données côté Client** option dans le **les paramètres de projet** boîte de dialogue.  
  
-   Dans **les paramètres de projet**, le **moteur de Migration de données côté Client** option est définie.  
  
    > [!NOTE]  
    > Le **moteur de Migration des données côté Client** réside à l’intérieur de l’application de SSMA et est donc pas dépendante de la disponibilité du Pack d’extension.  
  
**Migration des données côté serveur :**  
  
-   Lors de la migration des données côté serveur, le moteur se trouve sur la base de données cible. Il est installé par le biais du Pack d’extension. Pour plus d’informations sur la façon d’installer le pack d’extension, consultez [l’installation des composants de SSMA sur SQL Server](http://msdn.microsoft.com/en-us/cf2b724b-4ca7-470a-8dd7-fa95b1e060a4)  
  
-   Pour lancer la migration sur le côté serveur, sélectionnez le **moteur de Migration de données côté serveur** option dans le **les paramètres de projet** boîte de dialogue.  
  
## <a name="migrating-data-to-sql-server"></a>Migration de données vers SQL Server  
Migration de données sont une opération de chargement en masse qui déplace les lignes de données à partir des tables DB2 dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] tables dans des transactions. Le nombre de lignes chargées dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] dans chaque transaction est configuré dans les paramètres du projet.  
  
Pour afficher les messages de la migration, assurez-vous que le volet de résultats est visible. Sinon, à partir de la **vue** menu, sélectionnez **sortie**.  
  
**Pour migrer des données**  
  
1.  Vérifiez les éléments suivants :  
  
    -   Les fournisseurs de DB2 sont installés sur l’ordinateur qui est en cours d’exécution SSMA.  
  
    -   Vous avez synchronisé les objets convertis avec le [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] base de données.  
  
2.  Dans l’Explorateur de métadonnées de DB2, sélectionnez les objets qui contiennent les données que vous souhaitez migrer :  
  
    -   Pour migrer les données pour tous les schémas, sélectionnez la case à cocher à côté **schémas**.  
  
    -   Pour migrer des données ou omettre des tables individuelles, tout d’abord développer le schéma, développez **Tables**, puis activez ou désactivez la case à cocher en regard de la table.  
  
3.  Pour migrer des données, deux cas se présentent :  
  
    **Migration des données côté client :**  
  
    -   Pour effectuer des **Migration des données côté Client**, sélectionnez le **moteur de Migration de données côté Client** option dans le **les paramètres de projet** boîte de dialogue.  
  
    **Migration des données côté serveur :**  
  
    -   Avant d’effectuer la migration des données côté serveur, vérifiez :  
  
        1.  SSMA pour DB2 Extension Pack est installé sur l’instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)].  
  
        2.  Le service SQL Server Agent est en cours d’exécution sur l’instance de SQL Server.  
  
    -   Pour effectuer des **Migration des données côté serveur**, sélectionnez le **moteur de Migration de données côté serveur** option dans le **les paramètres de projet** boîte de dialogue.  
  
4.  Avec le bouton droit **schémas** dans l’Explorateur de métadonnées de DB2, puis cliquez sur **migrer des données**. Vous pouvez également migrer des données pour des objets individuels ou des catégories d’objets : cliquez sur l’objet ou son dossier parent ; Sélectionnez le **migrer des données** option.  
  
    > [!NOTE]  
    > Si SSMA pour DB2 Extension Pack n’est pas installé sur l’instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)]et si **moteur de Migration de données côté serveur** est sélectionnée, puis rencontré lors de la migration des données à la base de données cible, l’erreur suivante : « composants de Migration de données de SSMA sont introuvables sur le serveur SQL Server, migration de données côté serveur ne sera pas possible. Vérifiez si le Pack d’Extension est correctement installé ». Cliquez sur **Annuler** pour mettre fin à la migration des données.  
  
5.  Dans le **se connecter à DB2** boîte de dialogue, entrez les informations d’identification de connexion, puis cliquez sur **connexion**. Pour plus d’informations sur la connexion à DB2, consultez [la connexion à la base de données DB2 &#40;DB2ToSQL&#41;](../../ssma/db2/connecting-to-db2-database-db2tosql.md)  
  
    Pour se connecter à la base de données cible [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)], entrez les informations d’identification de connexion dans le **se connecter à SQL Server** boîte de dialogue, puis cliquez sur **connexion**. Pour plus d’informations sur la connexion à [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)], consultez [connexion à SQL Server](http://msdn.microsoft.com/en-us/b59803cb-3cc6-41cc-8553-faf90851410e)  
  
    Messages seront affichés dans le **sortie** volet. Une fois la migration terminée, le **rapport de Migration de données** s’affiche. Si aucune donnée n’a pas été migré, cliquez sur la ligne qui contient les erreurs, puis cliquez sur **détails**. Lorsque vous avez terminé avec le rapport, cliquez sur **fermer**. Pour plus d’informations sur le rapport de Migration de données, consultez [rapport de Migration de données (SSMA commun)](http://msdn.microsoft.com/en-us/bbfb9d88-5a98-4980-8d19-c5d78bd0d241)  
  
> [!NOTE]  
> Lors de l’édition de SQL Express est utilisée comme base de données cible, uniquement les migration de données côté client est autorisée et migration des données côté serveur n’est pas pris en charge.  
  
## <a name="see-also"></a>Voir aussi  
[Migration de données DB2 dans SQL Server &#40;DB2ToSQL&#41;](../../ssma/db2/migrating-db2-data-into-sql-server-db2tosql.md)  
  
