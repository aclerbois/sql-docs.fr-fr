---
title: Spécifier le mode de propagation des modifications des articles transactionnels | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
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
- transactional replication, propagation methods
ms.assetid: a10c5001-22cc-4667-8f0b-3d0818dca2e9
caps.latest.revision: 48
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: e1301da5fd36bf38a28bb4722c460642d65fa9b3
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="transactional-articles---specify-how-changes-are-propagated"></a>Articles transactionnels - Spécifier le mode de propagation des modifications
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  La réplication transactionnelle permet de préciser comment les modifications des données sont propagées entre le serveur de publication et les Abonnés. Pour chaque table publiée, vous pouvez spécifier l'une des quatre méthodes de propagation possibles d'une opération (INSERT, UPDATE ou DELETE) vers l'Abonné :  
  
-   Spécifier que la réplication transactionnelle doit générer un script puis appeler une procédure stockée pour propager les modifications aux Abonnés (option par défaut).  
  
-   Spécifier que la modification doit être propagée à l'aide d'une instruction INSERT, UPDATE ou DELETE (option par défaut pour les Abonnés non[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] ).  
  
-   Spécifier l'utilisation d'une procédure stockée personnalisée.  
  
-   Spécifier que cette action ne doit pas être effectuée sur les Abonnés. Les transactions de ce type ne sont pas répliquées.  
  
 Par défaut, la réplication transactionnelle propage les modifications vers les abonnés via un groupe de procédures stockées installées sur chaque abonné. Lorsqu'une opération Insert, Update ou Delete est effectuée sur une table du serveur de publication, elle est convertie en appel à une procédure stockée sur l'Abonné. La procédure stockée accepte des paramètres correspondant aux colonnes de la table, ce qui permet la modification de ces colonnes sur l'Abonné.  
  
 Pour définir la méthode de propagation des modifications de données des articles transactionnels, consultez [Définir la méthode de propagation des modifications de données des articles transactionnels](../../../relational-databases/replication/publish/set-the-propagation-method-for-data-changes-to-transactional-articles.md).  
  
## <a name="default-and-custom-stored-procedures"></a>Procédures stockées par défaut et personnalisées  
 Trois procédures sont créées par défaut par la réplication pour chaque article de table :  
  
-   **sp_MSins_\<** *nomdetable* **>**, qui gère les insertions.  
  
-   **sp_MSupd_\<** *nomdetable* **>**, qui gère les mises à jour.  
  
-   **sp_MSdel_\<** *nomdetable* **>**, qui gère les suppressions.  
  
 Le **\<***nom de table***>** utilisé dans la procédure varie en fonction de la méthode utilisée pour ajouter l’article à la publication et si la base de données d’abonnement contient une table au nom identique mais avec un propriétaire différent.  
  
 N'importe laquelle de ces procédures peut être remplacée par une procédure personnalisée que vous spécifiez lors de l'ajout d'un article à une publication. Les procédures personnalisées sont utilisées dans le cas où l'application exige une logique personnalisée, par exemple l'insertion de données dans une table d'audit lors de la mise à jour d'une ligne sur l'Abonné. Pour plus d'informations sur la définition de procédures stockées personnalisées, reportez-vous à la liste des rubriques ci-dessus.  
  
 Si vous utilisez les procédures de réplication par défaut ou les procédures personnalisées, vous devez également spécifier la syntaxe d'appel pour chaque procédure (la réplication sélectionne les valeurs par défaut si vous utilisez les procédures par défaut). La syntaxe d'appel détermine la structure des paramètres fournis à la procédure et la quantité d'informations envoyées à l'Abonné avec chaque modification de données. Pour plus d'informations, consultez la section « Syntaxe d'appel des procédures stockées », plus loin dans cette rubrique.  
  
### <a name="considerations-for-using-custom-stored-procedures"></a>Considérations sur l'utilisation des procédures stockées personnalisées  
 Les éléments suivants doivent être pris en compte lors de l'utilisation de procédures stockées personnalisées :  
  
-   Vous devez prendre en charge la logique de la procédure stockée ; [!INCLUDE[msCoName](../../../includes/msconame-md.md)] n'assure pas la prise en charge d'une logique personnalisée.  
  
-   Afin d'éviter les conflits avec les transactions utilisées par la réplication, évitez d'utiliser des transactions explicites dans les procédures personnalisées.  
  
-   Le schéma de l'Abonné est généralement identique à celui du serveur de publication mais il peut également représenter un sous-ensemble du schéma du serveur de publication si vous utilisez le filtrage de colonnes. Toutefois, si vous devez transformer le schéma lors du déplacement des données de telle sorte que le schéma de l'Abonné ne représente pas un sous-ensemble du schéma sur le serveur de publication, [!INCLUDE[ssISCurrent](../../../includes/ssiscurrent-md.md)] est la solution recommandée. Pour plus d’informations, consultez [SQL Server Integration Services](../../../integration-services/sql-server-integration-services.md).  
  
-   Si vous modifiez le schéma d'une table publiée, les procédures personnalisées doivent être régénérées. Pour plus d’informations, consultez [Régénérer des procédures transactionnelles personnalisées pour refléter des modifications de schéma](../../../relational-databases/replication/transactional/transactional-articles-regenerate-to-reflect-schema-changes.md).  
  
-   Si vous utilisez une valeur supérieure à 1 pour le paramètre **-SubscriptionStreams** de l’Agent de distribution, vérifiez que les mises à jour des colonnes clés primaires se sont correctement déroulées. Exemple :  
  
    ```  
    update ... set pk = 2 where pk = 1 -- update 1  
    update ... set pk = 3 where pk = 2 -- update 2  
    ```  
  
     Si l'Agent de distribution utilise plusieurs connexions, ces deux mises à jour peuvent être répliquées sur différentes connexions. Si la première mise à jour (update 1) est d'abord appliquée, cela ne pose aucun problème ; en revanche, si la deuxième mise à jour (update 2) est la première appliquée, elle retourne '0 ligne affectée' parce que la première mise à jour n'a pas encore été effectuée. Dans les procédures par défaut, cette situation génère une erreur si aucune ligne n'est affectée dans une mise à jour :  
  
    ```  
    if @@rowcount = 0  
        if @@microsoftversion>0x07320000  
            exec sys.sp_MSreplraiserror 20598  
    ```  
  
     La génération de l'erreur force l'Agent de distribution à retenter les mises à jour sur une seule connexion, lesquelles, dans ce cas, réussissent. Les procédures stockées personnalisées doivent inclure une logique similaire.  
  
### <a name="call-syntax-for-stored-procedures"></a>Syntaxe d'appel des procédures stockées  
 Vous avez le choix entre cinq options quant à la syntaxe employée pour appeler les procédures utilisées par la réplication transactionnelle :  
  
-   Syntaxe CALL. Peut être utilisée pour les insertions, les mises à jour et les suppressions. Par défaut, la réplication utilise cette syntaxe pour les insertions et les suppressions.  
  
-   Syntaxe SCALL. Peut être utilisée uniquement pour les mises à jour. Par défaut, la réplication utilise cette syntaxe pour les mises à jour.  
  
-   Syntaxe MCALL. Peut être utilisée uniquement pour les mises à jour.  
  
-   Syntaxe XCALL Peut être utilisée pour les mises à jour et les suppressions.  
  
-   VCALL. Utilisée pour les abonnements pouvant être mis à jour. À usage interne uniquement  
  
 Ces méthodes diffèrent par le volume de données propagé à l'Abonné. Par exemple, SCALL passe des valeurs uniquement pour les colonnes affectées par une mise à jour. XCALL, en revanche, exige toutes les colonnes (qu'elles soient affectées ou non par une mise à jour) et toutes les anciennes valeurs des données pour chaque colonne. La syntaxe SCALL convient généralement aux opérations de mise à jour sauf si votre application exige toutes les valeurs des données au cours d'une mise à jour, auquel cas vous pouvez utiliser XCALL.  
  
#### <a name="call-syntax"></a>Syntaxe CALL  
 Procédures stockées INSERT  
 Les procédures stockées qui gèrent des instructions INSERT recevront les valeurs insérées pour toutes les colonnes :  
  
```  
c1, c2, c3,... cn  
```  
  
 Procédures stockées UPDATE  
 Les procédures stockées gérant des instructions UPDATE recevront les valeurs mises à jour pour toutes les colonnes définies dans l'article, suivies des valeurs d'origine pour les colonnes clés primaire (aucune tentative ne sera faite pour déterminer les colonnes qui ont été modifiées) :  
  
```  
c1, c2, c3,... cn, pkc1, pkc2, pkc3,... pkcn  
```  
  
 Procédures stockées DELETE  
 Les procédures stockées qui gèrent des instructions DELETE recevront des valeurs pour les colonnes clés primaire :  
  
```  
pkc1, pkc2, pkc3,... pkcn  
```  
  
#### <a name="scall-syntax"></a>Syntaxe SCALL  
 Procédures stockées UPDATE  
 Les procédures stockées gérant des instructions UPDATE ne recevront les valeurs mises à jour que pour les colonnes qui ont été modifiées, suivies tout d'abord des valeurs d'origine des colonnes clés primaire, puis d'un paramètre de masque binaire (**binary(n)**) indiquant les colonnes modifiées. Dans l'exemple suivant, la colonne 2 (c2) n'a pas été modifiée :  
  
```  
c1, , c3,... cn, pkc1, pkc2, pkc3,... pkcn, bitmask  
```  
  
#### <a name="mcall-syntax"></a>Syntaxe MCALL  
 Procédures stockées UPDATE  
 Les procédures stockées gérant des instructions UPDATE recevront les valeurs mises à jour de toutes les colonnes définies dans l'article, suivies tout d'abord des valeurs d'origine des colonnes clés primaire, puis d'un paramètre de masque binaire (**binary(n)**) indiquant les colonnes modifiées :  
  
```  
c1, c2, c3,... cn, pkc1, pkc2, pkc3,... pkcn, bitmask  
```  
  
#### <a name="xcall-syntax"></a>Syntaxe XCALL  
 Procédures stockées UPDATE  
 Les procédures stockées gérant des instructions UPDATE recevront les valeurs d'origine (c'est-à-dire l'image avant) de toutes les colonnes définies dans l'article, suivies des valeurs mises à jour (image après) de ces mêmes colonnes :  
  
```  
old-c1, old-c2, old-c3,... old-cn, c1, c2, c3,... cn,  
```  
  
 Procédures stockées DELETE  
 Les procédures stockées qui gèrent des instructions DELETE recevront les valeurs d'origine (image avant) de toutes les colonnes définies dans l'article :  
  
```  
old-c1, old-c2, old-c3,... old-cn  
```  
  
> [!NOTE]  
>  Lorsque la syntaxe XCALL est utilisée, les valeurs de l'image avant des colonnes **text** et **image** sont censées être NULL.  
  
## <a name="examples"></a>Exemples  
 Les procédures suivantes représentent les procédures par défaut créées pour la `Vendor Table` dans la base de données exemple de [!INCLUDE[ssSampleDBCoShort](../../../includes/sssampledbcoshort-md.md)] .  
  
```  
--INSERT procedure using CALL syntax  
create procedure [sp_MSins_PurchasingVendor]   
  @c1 int,@c2 nvarchar(15),@c3 nvarchar(50),@c4 tinyint,@c5 bit,@c6 bit,@c7 nvarchar(1024),@c8 datetime  
as   
begin   
insert into [Purchasing].[Vendor]([VendorID]  
,[AccountNumber]  
,[Name]  
,[CreditRating]  
,[PreferredVendorStatus]  
,[ActiveFlag]  
,[PurchasingWebServiceURL]  
,[ModifiedDate])  
values (   
 @c1  
,@c2  
,@c3  
,@c4  
,@c5  
,@c6  
,@c7  
,@c8  
 )   
end  
go  
  
--UPDATE procedure using SCALL syntax  
create procedure [sp_MSupd_PurchasingVendor]   
 @c1 int = null,@c2 nvarchar(15) = null,@c3 nvarchar(50) = null,@c4 tinyint = null,@c5 bit = null,@c6 bit = null,@c7 nvarchar(1024) = null,@c8 datetime = null,@pkc1 int  
,@bitmap binary(2)  
as  
begin  
update [Purchasing].[Vendor] set   
 [AccountNumber] = case substring(@bitmap,1,1) & 2 when 2 then @c2 else [AccountNumber] end  
,[Name] = case substring(@bitmap,1,1) & 4 when 4 then @c3 else [Name] end  
,[CreditRating] = case substring(@bitmap,1,1) & 8 when 8 then @c4 else [CreditRating] end  
,[PreferredVendorStatus] = case substring(@bitmap,1,1) & 16 when 16 then @c5 else [PreferredVendorStatus] end  
,[ActiveFlag] = case substring(@bitmap,1,1) & 32 when 32 then @c6 else [ActiveFlag] end  
,[PurchasingWebServiceURL] = case substring(@bitmap,1,1) & 64 when 64 then @c7 else [PurchasingWebServiceURL] end  
,[ModifiedDate] = case substring(@bitmap,1,1) & 128 when 128 then @c8 else [ModifiedDate] end  
where [VendorID] = @pkc1  
if @@rowcount = 0  
    if @@microsoftversion>0x07320000  
        exec sp_MSreplraiserror 20598  
end  
go  
  
--DELETE procedure using CALL syntax  
create procedure [sp_MSdel_PurchasingVendor]   
  @pkc1 int  
as   
begin   
delete [Purchasing].[Vendor]  
where [VendorID] = @pkc1  
if @@rowcount = 0  
    if @@microsoftversion>0x07320000  
        exec sp_MSreplraiserror 20598  
end   
go  
```  
  
## <a name="see-also"></a> Voir aussi  
 [Article Options for Transactional Replication](../../../relational-databases/replication/transactional/article-options-for-transactional-replication.md)  
  
  
