---
title: Afficher les fonctions définies par l’utilisateur | Microsoft Docs
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.component: udf
ms.reviewer: ''
ms.suite: sql
ms.technology:
- dbe-udf
ms.tgt_pltfrm: ''
ms.topic: conceptual
f1_keywords:
- sql13.swb.udfproperties.general.f1
- sql13.swb.functionproperties.general.f1
helpviewer_keywords:
- displaying user-defined functions
- viewing user-defined functions
- user-defined functions [SQL Server], viewing
- status information [SQL Server], user-defined functions
ms.assetid: a45dfab5-6384-4311-b935-2e23a70c5c10
caps.latest.revision: 30
author: rothja
ms.author: jroth
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: f994d29dd2f994c2931992c6f222d91b0ae0dbee
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="view-user-defined-functions"></a>Afficher les fonctions définies par l'utilisateur
[!INCLUDE[tsql-appliesto-ss2008-asdb-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-asdb-xxxx-xxx-md.md)]
  Vous pouvez obtenir des informations sur la définition ou les propriétés d'une fonction définie par l'utilisateur dans [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] à l'aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] ou de [!INCLUDE[tsql](../../includes/tsql-md.md)]. Vous devrez peut-être examiner la définition de la fonction pour comprendre comment les données de celle-ci sont issues des tables source ou pour connaître les données définies par la fonction.  
  
> [!IMPORTANT]  
>  Si vous modifiez le nom d'un objet auquel une fonction fait référence, vous devez modifier la fonction pour que son texte reflète le nouveau nom de l'objet. Par conséquent, avant de renommer un objet, affichez les dépendances de l'objet pour savoir si des fonctions peuvent être concernées par la modification projetée.  
  
 **Dans cette rubrique**  
  
-   **Avant de commencer :**  
  
     [Sécurité](#Security)  
  
-   **Pour obtenir des informations sur une fonction, utilisez :**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="BeforeYouBegin"></a> Avant de commencer  
  
###  <a name="Security"></a> Sécurité  
  
####  <a name="Permissions"></a> Permissions  
 L’utilisation de **sys.sql_expression_dependencies** pour rechercher toutes les dépendances sur une fonction nécessite l’autorisation VIEW DEFINITION sur la base de données et l’autorisation SELECT sur **sys.sql_expression_dependencies** pour la base de données. Les définitions d'objets système, telles que celles retournées dans OBJECT_DEFINITION, sont visibles publiquement.  
  
##  <a name="SSMSProcedure"></a> Utilisation de SQL Server Management Studio  
  
#### <a name="to-show-a-user-defined-functions-properties"></a>Pour afficher les propriétés d'une fonction définies par l'utilisateur  
  
1.  Dans l' **Explorateur d'objets**, cliquez sur le signe plus (+) en regard de la base de données qui contient la fonction dont vous souhaitez afficher les propriétés, puis cliquez sur le signe plus (+) pour développer le dossier **Programmabilité** .  
  
2.  Cliquez sur le signe plus (+) pour développer le dossier **Fonctions** .  
  
3.  Cliquez sur le signe plus (+) pour développer le dossier qui contient la fonction dont vous souhaitez afficher les propriétés :  
  
    -   Fonction table  
  
    -   Fonction scalaire  
  
    -   Fonction d'agrégation  
  
4.  Cliquez avec le bouton droit sur la fonction dont vous voulez afficher les propriétés, puis sélectionnez **Propriétés**.  
  
     Les propriétés suivantes s’affichent dans la boîte de dialogue **Propriétés de la fonction –** *nom_fonction* .  
  
     **Sauvegarde de la base de données**  
     Nom de la base de données contenant cette fonction.  
  
     **Server**  
     Nom de l'instance actuelle du serveur.  
  
     **Utilisateur**  
     Nom de l'utilisateur de cette connexion.  
  
     **Date de création**  
     Affiche la date à laquelle la fonction a été créée.  
  
     **Exécuter en tant que**  
     Contexte d'exécution de la fonction.  
  
     **Nom**  
     Nom de la fonction actuelle.  
  
     **Schéma**  
     Affiche le schéma auquel appartient la fonction.  
  
     **Objet système**  
     Indique si la fonction est un objet système. Les valeurs sont True et False.  
  
     **Valeurs ANSI NULL**  
     Indique si l'objet a été créé avec l'option Valeurs ANSI NULL.  
  
     **Chiffré**  
     Indique si la fonction est chiffrée. Les valeurs sont True et False.  
  
     **Type de fonction**  
     Type de la fonction définie par l'utilisateur.  
  
     **Identificateur entre guillemets**  
     Indique si l'objet a été créé avec l'option Identificateurs entre guillemets.  
  
     **Lié(e) au schéma**  
     Indique si la fonction est liée au schéma. Les valeurs sont True et False. Pour plus d’informations sur les fonctions liées à un schéma, consultez la section SCHEMABINDING de [CREATE FUNCTION &#40;Transact-SQL&#41;](../../t-sql/statements/create-function-transact-sql.md).  
  
##  <a name="TsqlProcedure"></a> Utilisation de Transact-SQL  
  
#### <a name="to-get-the-definition-and-properties-of-a-function"></a>Pour obtenir la définition et les propriétés d'une fonction  
  
1.  Dans l' **Explorateur d'objets**, connectez-vous à une instance de [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  Dans la barre d'outils standard, cliquez sur **Nouvelle requête**.  
  
3.  Copiez et collez l'un des exemples suivants dans la fenêtre de requête, puis cliquez sur **Exécuter**.  
  
    ```  
    USE AdventureWorks2012;  
    GO  
    -- Get the function name, definition, and relevant properties  
    SELECT sm.object_id,   
       OBJECT_NAME(sm.object_id) AS object_name,   
       o.type,   
       o.type_desc,   
       sm.definition,  
       sm.uses_ansi_nulls,  
       sm.uses_quoted_identifier,  
       sm.is_schema_bound,  
       sm.execute_as_principal_id  
    -- using the two system tables sys.sql_modules and sys.objects  
    FROM sys.sql_modules AS sm  
    JOIN sys.objects AS o ON sm.object_id = o.object_id  
    -- from the function 'dbo.ufnGetProductDealerPrice'  
    WHERE sm.object_id = OBJECT_ID('dbo.ufnGetProductDealerPrice')  
    ORDER BY o.type;  
    GO  
  
    ```  
  
    ```  
    USE AdventureWorks2012;  
    GO  
    -- Get the definition of the function dbo.ufnGetProductDealerPrice  
    SELECT OBJECT_DEFINITION (OBJECT_ID('dbo.ufnGetProductDealerPrice')) AS ObjectDefinition;  
    GO  
    ```  
  
 Pour plus d’informations, consultez [sys.sql_modules &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-sql-modules-transact-sql.md) et [OBJECT_DEFINITION &#40;Transact-SQL&#41;](../../t-sql/functions/object-definition-transact-sql.md).  
  
#### <a name="to-get-the-dependencies-of-a-function"></a>Pour obtenir les dépendances d'une fonction  
  
1.  Dans l' **Explorateur d'objets**, connectez-vous à une instance de [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  Dans la barre d'outils standard, cliquez sur **Nouvelle requête**.  
  
3.  Copiez et collez l'exemple suivant dans la fenêtre de requête, puis cliquez sur **Exécuter**.  
  
    ```  
    USE AdventureWorks2012;  
    GO  
    -- Get all of the dependency information  
    SELECT OBJECT_NAME(sed.referencing_id) AS referencing_entity_name,   
        o.type_desc AS referencing_desciption,   
        COALESCE(COL_NAME(sed.referencing_id, sed.referencing_minor_id), '(n/a)') AS referencing_minor_id,   
        sed.referencing_class_desc, sed.referenced_class_desc,  
        sed.referenced_server_name, sed.referenced_database_name, sed.referenced_schema_name,  
        sed.referenced_entity_name,   
        COALESCE(COL_NAME(sed.referenced_id, sed.referenced_minor_id), '(n/a)') AS referenced_column_name,  
        sed.is_caller_dependent, sed.is_ambiguous  
    -- from the two system tables sys.sql_expression_dependencies and sys.object  
    FROM sys.sql_expression_dependencies AS sed  
    INNER JOIN sys.objects AS o ON sed.referencing_id = o.object_id  
    -- on the function dbo.ufnGetProductDealerPrice  
    WHERE sed.referencing_id = OBJECT_ID('dbo.ufnGetProductDealerPrice');  
    GO  
    ```  
  
 Pour plus d’informations, consultez [sys.sql_expression_dependencies &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-sql-expression-dependencies-transact-sql.md) et [sys.objects &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-objects-transact-sql.md).  
  
  
