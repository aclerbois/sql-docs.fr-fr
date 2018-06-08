---
title: Créer des statistiques | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.component: statistics
ms.reviewer: ''
ms.suite: sql
ms.technology:
- dbe-statistics
ms.tgt_pltfrm: ''
ms.topic: conceptual
f1_keywords:
- sql13.swb.stat.properties.f1
- sql13.swb.statistics.filter.f1
- sql13.swb.stat.columns.f1
- sql13.swb.statistics.propertis.f1
helpviewer_keywords:
- creating statistics
- statistics [SQL Server], creating
ms.assetid: 95a455fb-664d-4c95-851e-c6b62d7ebe04
caps.latest.revision: 9
author: MikeRayMSFT
ms.author: mikeray
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: f4a06d45ab3fa089f07e31ac8b8fd9a3162e820f
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="create-statistics"></a>Créer des statistiques
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
  Vous pouvez créer des statistiques d'optimisation de requête sur une ou plusieurs colonnes d'une table ou d'une vue indexée dans [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] à l'aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] ou de [!INCLUDE[tsql](../../includes/tsql-md.md)]. Pour la plupart des requêtes, l'optimiseur de requête génère déjà les statistiques utiles à un plan de requête de haute qualité ; dans certains cas, vous devez créer des statistiques supplémentaires.  
  
 **Dans cette rubrique**  
  
-   **Avant de commencer :**  
  
     [Limitations et restrictions](#Restrictions)  
  
     [Sécurité](#Security)  
  
-   **Pour créer des statistiques, utilisez :**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="BeforeYouBegin"></a> Avant de commencer  
  
###  <a name="Restrictions"></a> Limitations et restrictions  
  
-   Avant de créer des statistiques avec l'instruction CREATE STATISTICS, vérifiez que l'option AUTO_CREATE_STATISTICS est définie au niveau de la base de données. Cela garantit que l'optimiseur de requête continue de créer régulièrement des statistiques de colonnes uniques pour les colonnes de prédicat de requête.  
  
-   Vous pouvez afficher jusqu'à 32 colonnes par objet de statistiques.  
  
-   Vous ne pouvez pas supprimer, renommer ni modifier la définition d'une colonne de table définie dans un prédicat de statistiques filtrées.  
  
###  <a name="Security"></a> Sécurité  
  
####  <a name="Permissions"></a> Permissions  
 Nécessite que l’utilisateur soit le propriétaire de la table ou de la vue indexée ou qu’il soit membre d’un des rôles suivants : rôle serveur fixe **sysadmin** , rôle de base de données fixe **db_owner** ou rôle de base de données fixe **db_ddladmin** .  
  
##  <a name="SSMSProcedure"></a> Utilisation de SQL Server Management Studio  
  
#### <a name="to-create-statistics"></a>Pour créer des statistiques  
  
1.  Dans l' **Explorateur d'objets**, cliquez sur le signe plus (+) pour développer la base de données dans laquelle vous souhaitez créer une nouvelle statistique.  
  
2.  Cliquez sur le signe plus (+) pour développer le dossier **Tables** .  
  
3.  Cliquez sur le signe plus (+) pour développer la table dans laquelle vous souhaitez créer une statistique.  
  
4.  Cliquez avec le bouton droit sur le dossier **Statistiques** et sélectionnez **Nouvelles statistiques**.  
  
     Les propriétés suivantes s’affichent dans la page **Général** de la boîte de dialogue **Nouvelles statistiques sur la table***nom_table*.  
  
     **Nom de la table**  
     Affiche le nom de la table décrite par les statistiques.  
  
     **Nom des statistiques**  
     Spécifie le nom de l'objet de base de données dans lequel les statistiques sont stockées.  
  
     **Colonnes de statistiques**  
     Cette grille montre les colonnes décrites par ce jeu de statistiques. Toutes les valeurs contenues dans la grille sont en lecture seule.  
  
     **Nom**  
     Affiche le nom de la colonne décrite par les statistiques. Cela peut être une colonne individuelle ou une combinaison de colonnes dans une table individuelle.  
  
     **Type de données**  
     Indique le type de données des colonnes décrites par les statistiques.  
  
     **Taille**  
     Indique la taille du type de données de chaque colonne.  
  
     **Identity**  
     Indique une colonne d'identité lorsque l'option est activée.  
  
     **Null autorisé**  
     Indique si la colonne accepte les valeurs NULL.  
  
     **Ajouter**  
     Permet d'ajouter des colonnes supplémentaires de la table dans la grille des statistiques.  
  
     **Supprimer**  
     Permet de supprimer la colonne sélectionnée de la grille des statistiques.  
  
     **Monter**  
     Permet de déplacer la colonne sélectionnée vers un emplacement antérieur dans la grille des statistiques. L'emplacement dans la grille peut affecter de manière significative l'utilité des statistiques.  
  
     **Descendre**  
     Permet de déplacer la colonne sélectionnée vers un emplacement ultérieur dans la grille des statistiques.  
  
     **Les statistiques de ces colonnes ont été mises à jour la dernière fois le :**  
     Indique l'ancienneté des statistiques. Les statistiques ont plus de valeur lorsqu'elles sont actuelles. Mettez à jour les statistiques après des modifications importantes des données ou après l'ajout de données atypiques. Les statistiques de tables dont la distribution des données est cohérente peuvent être mises à jour moins souvent.  
  
     **Mettre à jour les statistiques pour ces colonnes**  
     Activez cette option pour mettre à jour les statistiques lors de la fermeture de la boîte de dialogue.  
  
     La propriété suivante s’affiche dans la page **Filtre** de la boîte de dialogue **Nouvelles statistiques sur la table***nom_table*.  
  
     **Expression de filtre**  
     Définit quelles lignes de données inclure dans les statistiques filtrées. Par exemple, `Production.ProductSubcategoryID IN ( 1,2,3 )`  
  
5.  Dans la boîte de dialogue **Nouvelles statistiques sur la table***nom_table*, dans la page **Général**, cliquez sur **Ajouter**.  
  
     Les propriétés suivantes s'affichent dans la boîte de dialogue **Sélectionner les colonnes** . Ces informations sont en lecture seule.  
  
     **Nom**  
     Affiche le nom de la colonne décrite par les statistiques. Cela peut être une colonne individuelle ou une combinaison de colonnes dans une table individuelle.  
  
     **Type de données**  
     Indique le type de données des colonnes décrites par les statistiques.  
  
     **Taille**  
     Indique la taille du type de données de chaque colonne.  
  
     **Identity**  
     Indique une colonne d'identité lorsque l'option est cochée.  
  
     **Allow NULLs**  
     Indique si la colonne accepte les valeurs NULL.  
  
6.  Dans la boîte de dialogue **Sélectionner les colonnes** , activez la ou les cases à cocher de chaque colonne pour laquelle vous voulez créer une statistique, puis cliquez sur **OK**.  
  
7.  Dans la boîte de dialogue **Nouvelles statistiques sur la table***nom_table*, cliquez sur **OK**.  
  
##  <a name="TsqlProcedure"></a> Utilisation de Transact-SQL  
  
#### <a name="to-create-statistics"></a>Pour créer des statistiques  
  
1.  Dans l' **Explorateur d'objets**, connectez-vous à une instance de [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  Dans la barre d'outils standard, cliquez sur **Nouvelle requête**.  
  
3.  Copiez et collez l'exemple suivant dans la fenêtre de requête, puis cliquez sur **Exécuter**.  
  
    ```  
    USE AdventureWorks2012;   
    GO  
    -- Create new statistic object called ContactMail1  
    -- on the BusinessEntityID and EmailPromotion columns in the Person.Person table.   
  
    CREATE STATISTICS ContactMail1  
        ON Person.Person (BusinessEntityID, EmailPromotion);   
    GO  
    ```  
  
4.  Les statistiques créées ci-dessus améliorent potentiellement les résultats de la requête suivante.  
  
    ```  
    USE AdventureWorks2012;   
    GO  
    SELECT LastName, FirstName  
    FROM Person.Person  
    WHERE EmailPromotion = 2  
    ORDER BY LastName, FirstName;   
    GO  
    ```  
  
 Pour plus d’informations, consultez [CREATE STATISTICS &#40;Transact-SQL&#41;](../../t-sql/statements/create-statistics-transact-sql.md).  
  
  
