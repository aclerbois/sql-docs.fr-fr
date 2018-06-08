---
title: Statistiques pour les tables optimisées en mémoire | Microsoft Docs
ms.custom: ''
ms.date: 10/23/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.component: in-memory-oltp
ms.reviewer: ''
ms.suite: sql
ms.technology: in-memory-oltp
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: e644766d-1d1c-43d7-83ff-8ccfe4f3af9f
caps.latest.revision: 18
author: MightyPen
ms.author: genemi
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: b1c664857e75b8f647b02905a26effb8e8b2c5e2
ms.sourcegitcommit: ee661730fb695774b9c483c3dd0a6c314e17ddf8
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/19/2018
---
# <a name="statistics-for-memory-optimized-tables"></a>Statistiques pour les tables optimisées en mémoire
[!INCLUDE[appliesto-ss-asdb-xxxx-xxx-md](../../includes/appliesto-ss-asdb-xxxx-xxx-md.md)]

  L'optimiseur de requête utilise des statistiques sur les colonnes dans l'optique de créer des plans de requête qui améliorent les performances des requêtes. Les statistiques sont collectées dans les tables de la base de données et stockées dans les métadonnées de la base de données.  
  
 Les statistiques sont créées automatiquement, mais elles peuvent également être créées manuellement. Par exemple, des statistiques sont créées automatiquement pour les colonnes clés d'index lorsque l'index est créé. Pour plus d'informations sur la création de statistiques, consultez [Statistics](../../relational-databases/statistics/statistics.md).  
  
 Les données de table sont généralement modifiées au fil du temps, à mesure que des lignes sont insérées, mises à jour et supprimées. Cela signifie que les statistiques doivent être mises à jour régulièrement. Par défaut, les statistiques sur les tables sont mises à jour automatiquement lorsque l’optimiseur de requête détermine qu’elles sont peut-être obsolètes.  
  
 Considérations sur les statistiques relatives aux tables optimisées en mémoire :  
  
-   À compter de SQL Server 2016 et de [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)], la mise à jour automatique des statistiques est prise en charge pour les tables optimisées en mémoire, lorsque vous utilisez un niveau de compatibilité d’au moins 130 de la base de données. Consultez [Niveau de compatibilité ALTER DATABASE (Transact-SQL)](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md). Si une base de données comporte des tables qui ont été créées à l’aide d’un niveau de compatibilité inférieur, les statistiques doivent être mises à jour manuellement une fois, pour activer la mise à jour automatique des statistiques par la suite.
  
-   Pour les procédures stockées compilées en mode natif, les plans d’exécution de requêtes dans la procédure sont optimisés lorsque la procédure est compilée, ce qui se produit au moment de la création. Ils ne sont pas recompilés automatiquement lors de la mise à jour des statistiques. Par conséquent, les tables doivent contenir un jeu représentatif de données avant la création des procédures.  
  
-   Les procédures stockées compilées en mode natif peuvent être recompilées manuellement à l’aide de [sp_recompile (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-recompile-transact-sql.md), et elles sont automatiquement recompilées si la base de données est mise hors connexion, puis remise en ligne, ou en cas de basculement de la base de données ou d’un redémarrage du serveur.  
  
## <a name="enabling-automatic-update-of-statistics-in-existing-tables"></a>Activation de la mise à jour automatique des statistiques dans les tables existantes

Lorsque des tables sont créées dans une base de données dotée d’un niveau de compatibilité d’au moins 130, la mise à jour automatique des statistiques est activée pour toutes les statistiques sur la table, et aucune action supplémentaire n’est nécessaire.

Si une base de données comporte des tables optimisées en mémoire qui ont été créées dans une version antérieure de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou sous un niveau de compatibilité inférieur à 130, les statistiques doivent être mises à jour manuellement une fois, pour activer la mise à jour automatique par la suite.

Pour activer la mise à jour automatique des statistiques pour des tables optimisées en mémoire qui ont été créées sous un niveau de compatibilité plus ancien, procédez comme suit :

1. Actualisez le niveau de compatibilité de la base de données : `ALTER DATABASE CURRENT SET COMPATIBILITY_LEVEL=130`

2. Mettez à jour manuellement les statistiques des tables optimisées en mémoire. Un exemple de script équivalent est fourni ci-dessous.

3. Recompilez manuellement les procédures stockées compilées en mode natif pour bénéficier des statistiques mises à jour.

*Script unique pour les statistiques :* pour les tables optimisées en mémoire qui ont été créées sous un niveau de compatibilité inférieur, vous pouvez exécuter le script Transact-SQL suivant une fois pour mettre à jour les statistiques de toutes les tables optimisées en mémoire et activer la mise à jour automatique des statistiques par la suite (en supposant que le paramètre AUTO_UPDATE_STATISTICS est activé pour la base de données) :

```
-- Assuming AUTO_UPDATE_STATISTICS is already ON for your database:
-- ALTER DATABASE CURRENT SET AUTO_UPDATE_STATISTICS ON;

ALTER DATABASE CURRENT SET COMPATIBILITY_LEVEL = 130;
GO
DECLARE @sql NVARCHAR(MAX) = N'';
SELECT
      @sql += N'UPDATE STATISTICS '
         + quotename(schema_name(t.schema_id))
         + N'.'
         + quotename(t.name)
         + ';' + CHAR(13) + CHAR(10)
   FROM sys.tables AS t
   WHERE t.is_memory_optimized = 1 AND 
        t.object_id IN (SELECT object_id FROM sys.stats WHERE no_recompute=1)
;
EXECUTE sp_executesql @sql;
GO
-- Each row appended to @sql looks roughly like:
-- UPDATE STATISTICS [dbo].[MyMemoryOptimizedTable];
```

*Vérifiez que la mise à jour automatique est activée :* le script suivant vérifie si la mise à jour automatique des statistiques des tables optimisées en mémoire est activée. Après l’exécution du script précédent, `1` est retourné dans la colonne `auto-update enabled` pour tous les objets de statistiques.

```
SELECT 
    quotename(schema_name(o.schema_id)) + N'.' + quotename(o.name) AS [table],
    s.name AS [statistics object],
    1-s.no_recompute AS [auto-update enabled]
FROM sys.stats s JOIN sys.tables o ON s.object_id=o.object_id
WHERE o.is_memory_optimized=1
```

## <a name="guidelines-for-deploying-tables-and-procedures"></a>Instructions pour le déploiement des tables et des procédures  
 Pour garantir que l’optimiseur de requête dispose de statistiques à jour lorsque vous créez des plans de requête, déployez les tables optimisées en mémoire et les procédures stockées compilées en mode natif qui accèdent à ces tables à l’aide des quatre étapes suivantes :  
  
1.  Veillez à ce que la base de données ait un niveau de compatibilité d’au moins 130. Consultez [Niveau de compatibilité ALTER DATABASE (Transact-SQL)](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md).

2.  Créez des tables et des index. Les index doivent être spécifiés inline dans les instructions **CREATE TABLE** .  
  
3.  Chargez des données dans les tables.  
  
4.  Créez des procédures stockées qui accèdent aux tables.  
  
 La création de procédures stockées compilées en mode natif après que vous avez chargé les données garantit que l’optimiseur dispose de statistiques pour les tables optimisées en mémoire. De cette façon, les plans de requête sont efficaces lorsque la procédure est compilée.  

## <a name="see-also"></a> Voir aussi  
 [Tables optimisées en mémoire](../../relational-databases/in-memory-oltp/memory-optimized-tables.md)  
  
  
