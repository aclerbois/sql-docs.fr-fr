---
title: OBJECTPROPERTY (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.component: t-sql|functions
ms.reviewer: ''
ms.suite: sql
ms.technology: t-sql
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- OBJECTPROPERTY
- OBJECTPROPERTY_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- displaying schema-scoped object information
- viewing schema-scoped object information
- OBJECTPROPERTY function
- schema-scoped objects [SQL Server]
- objects [SQL Server], schema-scoped
ms.assetid: 27569888-f8b5-4cec-a79f-6ea6d692b4ae
caps.latest.revision: 81
author: edmacauley
ms.author: edmaca
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: b6c05f2cc886070dee8c4d0ff586904cdfc964f8
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="objectproperty-transact-sql"></a>OBJECTPROPERTY (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-all-md](../../includes/tsql-appliesto-ss2008-all-md.md)]

  Retourne des informations concernant les objets étendus aux schémas dans la base de données actuelle. Pour obtenir la liste de tous les objets délimités aux schémas, consultez [sys.objects &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-objects-transact-sql.md). Cette fonction ne peut pas être utilisée pour les objets non étendus aux schémas, tels que les déclencheurs DDL et les notifications d'événements.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
OBJECTPROPERTY ( id , property )   
```  
  
## <a name="arguments"></a>Arguments  
 *id*  
 Expression représentant l'ID de l'objet dans la base de données active. *id* est de type **int** et est considéré comme un objet étendu aux schémas dans le contexte de la base de données active.  
  
 *property*  
 Expression représentant les informations devant être retournées pour l’objet spécifié par *id*. *property* peut avoir l’une des valeurs suivantes.  
  
> [!NOTE]  
>  Sauf indication contraire, la valeur NULL est retournée lorsque *property* n’est pas un nom de propriété valide, lorsque *id* n’est pas un ID d’objet valide, lorsque *id* est un type d’objet qui n’est pas pris en charge pour la propriété *property* spécifiée ou lorsque l’appelant n’est pas autorisé à consulter les métadonnées de l’objet.  
  
|Nom de la propriété|Type d'objet|Description et valeurs retournées|  
|-------------------|-----------------|-------------------------------------|  
|CnstIsClustKey|Contrainte|Contrainte PRIMARY KEY avec un index cluster.<br /><br /> 1 = True<br /><br /> 0 = False|  
|CnstIsColumn|Contrainte|Contrainte CHECK, DEFAULT ou FOREIGN KEY sur une seule colonne.<br /><br /> 1 = True<br /><br /> 0 = False|  
|CnstIsDeleteCascade|Contrainte|Contrainte FOREIGN KEY avec l'option ON DELETE CASCADE.<br /><br /> 1 = True<br /><br /> 0 = False|  
|CnstIsDisabled|Contrainte|Contrainte désactivée.<br /><br /> 1 = True<br /><br /> 0 = False|  
|CnstIsNonclustKey|Contrainte|Contrainte PRIMARY KEY ou UNIQUE avec index non-cluster.<br /><br /> 1 = True<br /><br /> 0 = False|  
|CnstIsNotRepl|Contrainte|La contrainte est définie avec les mots clés NOT FOR REPLICATION.<br /><br /> 1 = True<br /><br /> 0 = False|  
|CnstIsNotTrusted|Contrainte|La contrainte a été activée sans vérifier les lignes existantes. Elle peut ne pas s'appliquer à toutes les lignes.<br /><br /> 1 = True<br /><br /> 0 = False|  
|CnstIsUpdateCascade|Contrainte|Contrainte FOREIGN KEY avec l'option ON UPDATE CASCADE.<br /><br /> 1 = True<br /><br /> 0 = False|  
|ExecIsAfterTrigger|Déclencheur|Déclencheur AFTER.<br /><br /> 1 = True<br /><br /> 0 = False|  
|ExecIsAnsiNullsOn|Fonction [!INCLUDE[tsql](../../includes/tsql-md.md)], procédure [!INCLUDE[tsql](../../includes/tsql-md.md)], déclencheur [!INCLUDE[tsql](../../includes/tsql-md.md)], vue|Définition de valeurs ANSI NULL lors de la création.<br /><br /> 1 = True<br /><br /> 0 = False|  
|ExecIsDeleteTrigger|Déclencheur|Déclencheur DELETE.<br /><br /> 1 = True<br /><br /> 0 = False|  
|ExecIsFirstDeleteTrigger|Déclencheur|Premier déclencheur activé lorsqu'une instruction DELETE est exécutée sur la table.<br /><br /> 1 = True<br /><br /> 0 = False|  
|ExecIsFirstInsertTrigger|Déclencheur|Premier déclencheur activé lorsqu'une instruction INSERT est exécutée sur la table.<br /><br /> 1 = True<br /><br /> 0 = False|  
|ExecIsFirstUpdateTrigger|Déclencheur|Premier déclencheur activé lorsqu'une instruction UPDATE est exécutée sur la table.<br /><br /> 1 = True<br /><br /> 0 = False|  
|ExecIsInsertTrigger|Déclencheur|Déclencheur INSERT.<br /><br /> 1 = True<br /><br /> 0 = False|  
|ExecIsInsteadOfTrigger|Déclencheur|Déclencheur INSTEAD OF.<br /><br /> 1 = True<br /><br /> 0 = False|  
|ExecIsLastDeleteTrigger|Déclencheur|Dernier déclencheur activé lorsqu'une instruction DELETE est exécutée sur la table.<br /><br /> 1 = True<br /><br /> 0 = False|  
|ExecIsLastInsertTrigger|Déclencheur|Dernier déclencheur activé lorsqu'une instruction INSERT est exécutée sur la table.<br /><br /> 1 = True<br /><br /> 0 = False|  
|ExecIsLastUpdateTrigger|Déclencheur|Dernier déclencheur activé lorsqu'une instruction UPDATE est exécutée sur la table.<br /><br /> 1 = True<br /><br /> 0 = False|  
|ExecIsQuotedIdentOn|Fonction [!INCLUDE[tsql](../../includes/tsql-md.md)], procédure [!INCLUDE[tsql](../../includes/tsql-md.md)], déclencheur [!INCLUDE[tsql](../../includes/tsql-md.md)], vue|Définition de QUOTED_IDENTIFIER lors de la création.<br /><br /> 1 = True<br /><br /> 0 = False|  
|ExecIsStartup|Procédure|Procédure de démarrage.<br /><br /> 1 = True<br /><br /> 0 = False|  
|ExecIsTriggerDisabled|Déclencheur|Déclencheur désactivé.<br /><br /> 1 = True<br /><br /> 0 = False|  
|ExecIsTriggerNotForRepl|Déclencheur|Déclencheur défini comme NOT FOR REPLICATION.<br /><br /> 1 = True<br /><br /> 0 = False|  
|ExecIsUpdateTrigger|Déclencheur|Déclencheur UPDATE.<br /><br /> 1 = True<br /><br /> 0 = False|  
|ExecIsWithNativeCompilation|Procédure [!INCLUDE[tsql](../../includes/tsql-md.md)]|**S'applique à**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].<br /><br /> Procédure compilée en mode natif.<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> Type de données de base : **int**|  
|HasAfterTrigger|Table, vue|La table ou la vue comporte un déclencheur AFTER.<br /><br /> 1 = True<br /><br /> 0 = False|  
|HasDeleteTrigger|Table, vue|La table ou la vue comporte un déclencheur DELETE.<br /><br /> 1 = True<br /><br /> 0 = False|  
|HasInsertTrigger|Table, vue|La table ou la vue comporte un déclencheur INSERT.<br /><br /> 1 = True<br /><br /> 0 = False|  
|HasInsteadOfTrigger|Table, vue|La table ou la vue comporte un déclencheur INSTEAD OF.<br /><br /> 1 = True<br /><br /> 0 = False|  
|HasUpdateTrigger|Table, vue|La table ou la vue comporte un déclencheur UPDATE.<br /><br /> 1 = True<br /><br /> 0 = False|  
|IsAnsiNullsOn|Fonction [!INCLUDE[tsql](../../includes/tsql-md.md)], procédure [!INCLUDE[tsql](../../includes/tsql-md.md)], table, déclencheur [!INCLUDE[tsql](../../includes/tsql-md.md)], vue|Spécifie que le paramètre d'option ANSI NULLS de la table a la valeur ON. Cela signifie que toutes les comparaisons avec une valeur nulle produisent la valeur UNKNOWN. Ce paramètre s'applique à l'ensemble des expressions dans la définition de la table, y compris les contraintes et les colonnes calculées, aussi longtemps que la table existe.<br /><br /> 1 = True<br /><br /> 0 = False|  
|IsCheckCnst|Tout objet étendu aux schémas|Contrainte CHECK.<br /><br /> 1 = True<br /><br /> 0 = False|  
|IsConstraint|Tout objet étendu aux schémas|Contrainte CHECK, DEFAULT ou FOREIGN KEY à une seule colonne sur une colonne ou une table.<br /><br /> 1 = True<br /><br /> 0 = False|  
|IsDefault|Tout objet étendu aux schémas|**S'applique à**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].<br /><br /> Valeur par défaut associée.<br /><br /> 1 = True<br /><br /> 0 = False|  
|IsDefaultCnst|Tout objet étendu aux schémas|Contrainte DEFAULT.<br /><br /> 1 = True<br /><br /> 0 = False|  
|IsDeterministic|Fonction, vue|Propriété de déterminisme de la fonction ou de la vue.<br /><br /> 1 = Déterministe<br /><br /> 0 = Non déterministe|  
|IsEncrypted|Fonction [!INCLUDE[tsql](../../includes/tsql-md.md)], procédure [!INCLUDE[tsql](../../includes/tsql-md.md)], table, déclencheur [!INCLUDE[tsql](../../includes/tsql-md.md)], vue|Indique que le texte d'origine provenant de l'instruction du module a été converti dans un format d'obfuscation. La sortie générée par l'obfuscation n'est pas visible directement dans les affichages catalogue de [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)]. Les utilisateurs n’ayant pas accès aux tables système ou aux fichiers de base de données ne peuvent pas récupérer le texte obscurci. Le texte est cependant à la disposition des utilisateurs qui peuvent accéder aux tables système via le [port DAC](../../database-engine/configure-windows/diagnostic-connection-for-database-administrators.md) ou accéder directement aux fichiers de base de données. Les utilisateurs qui peuvent associer un débogueur au processus serveur peuvent également récupérer la procédure d'origine de la mémoire au moment de l'exécution.<br /><br /> 1 = Chiffrée<br /><br /> 0 = Non chiffrée<br /><br /> Type de données de base : **int**|  
|IsExecuted|Tout objet étendu aux schémas|Objet pouvant être exécuté (vue, procédure, fonction ou déclencheur).<br /><br /> 1 = True<br /><br /> 0 = False|  
|IsExtendedProc|Tout objet étendu aux schémas|Procédure étendue.<br /><br /> 1 = True<br /><br /> 0 = False|  
|IsForeignKey|Tout objet étendu aux schémas|Contrainte FOREIGN KEY.<br /><br /> 1 = True<br /><br /> 0 = False|  
|IsIndexed|Table, vue|Table ou vue comportant un index.<br /><br /> 1 = True<br /><br /> 0 = False|  
|IsIndexable|Table, vue|Table ou vue pour laquelle un index peut être créé.<br /><br /> 1 = True<br /><br /> 0 = False|  
|IsInlineFunction|Fonction|Fonction Inline.<br /><br /> 1 = Fonction Inline<br /><br /> 0 = Fonction non Inline|  
|IsMSShipped|Tout objet étendu aux schémas|Objet créé durant l'installation de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].<br /><br /> 1 = True<br /><br /> 0 = False|  
|IsPrimaryKey|Tout objet étendu aux schémas|Contrainte PRIMARY KEY.<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> NULL = N'est pas une fonction, ou identificateur de l'objet non valide.|  
|IsProcedure|Tout objet étendu aux schémas|Procédure.<br /><br /> 1 = True<br /><br /> 0 = False|  
|IsQuotedIdentOn|Fonction [!INCLUDE[tsql](../../includes/tsql-md.md)], procédure [!INCLUDE[tsql](../../includes/tsql-md.md)], table, déclencheur [!INCLUDE[tsql](../../includes/tsql-md.md)], vue, contrainte CHECK, définition DEFAULT|Spécifie que le paramètre d'identificateur entre guillemets pour l'objet est ON. Cela signifie que des guillemets doubles délimitent les identificateurs dans toutes les expressions impliquées dans la définition de l'objet.<br /><br /> 1 = activé<br /><br /> 0 = désactivé|  
|IsQueue|Tout objet étendu aux schémas|File d'attente Service Broker<br /><br /> 1 = True<br /><br /> 0 = False|  
|IsReplProc|Tout objet étendu aux schémas|Procédure de réplication.<br /><br /> 1 = True<br /><br /> 0 = False|  
|IsRule|Tout objet étendu aux schémas|Règle liée.<br /><br /> 1 = True<br /><br /> 0 = False|  
|IsScalarFunction|Fonction|Fonction scalaire.<br /><br /> 1 = Fonction scalaire<br /><br /> 0 = Fonction non scalaire|  
|IsSchemaBound|Fonction, vue|Fonction ou vue liée à un schéma, créée à l'aide de SCHEMABINDING.<br /><br /> 1 = Fonction liée à un schéma<br /><br /> 0 = Non liée à un schéma|  
|IsSystemTable|Table de charge de travail|Table système.<br /><br /> 1 = True<br /><br /> 0 = False|  
|IsTable|Table de charge de travail|Table.<br /><br /> 1 = True<br /><br /> 0 = False|  
|IsTableFunction|Fonction|Fonction table.<br /><br /> 1 = Fonction table<br /><br /> 0 = Fonction non-table|  
|IsTrigger|Tout objet étendu aux schémas|Déclencheur.<br /><br /> 1 = True<br /><br /> 0 = False|  
|IsUniqueCnst|Tout objet étendu aux schémas|Contrainte UNIQUE.<br /><br /> 1 = True<br /><br /> 0 = False|  
|IsUserTable|Table de charge de travail|Table définie par l'utilisateur.<br /><br /> 1 = True<br /><br /> 0 = False|  
|IsView|Affichage|Vue.<br /><br /> 1 = True<br /><br /> 0 = False|  
|OwnerId|Tout objet étendu aux schémas|Propriétaire de l'objet.<br /><br /> **Remarque :** Le propriétaire du schéma n’est pas nécessairement le propriétaire de l’objet. Par exemple, les objets enfants (ceux où *parent_object_id* est non-NULL) retournent toujours le même ID de propriétaire que leur parent.<br /><br /> Non NULL = ID utilisateur de base de données du propriétaire de l'objet.|  
|TableDeleteTrigger|Table de charge de travail|La table comporte un déclencheur DELETE.<br /><br /> >1 = ID du premier déclencheur du type spécifié.|  
|TableDeleteTriggerCount|Table de charge de travail|La table comporte le nombre de déclencheurs DELETE spécifié.<br /><br /> >0 = Nombre de déclencheurs DELETE.|  
|TableFullTextMergeStatus|Table de charge de travail|**S'applique à**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].<br /><br /> Indique s'il s'agit d'une table qui a un index de recherche en texte intégral qui est en cours de fusion.<br /><br /> 0 = La table n'a pas d'index de recherche en texte intégral ou l'index de recherche en texte intégral n'est pas en cours de fusion.<br /><br /> 1 = L'index de recherche en texte intégral est en cours de fusion.|  
|TableFullTextBackgroundUpdateIndexOn|Table de charge de travail|**S'applique à**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].<br /><br /> Un index de mise à jour d'arrière-plan de texte intégral est activé (suivi des modifications automatiques) pour la table.<br /><br /> 1 = TRUE<br /><br /> 0 = FALSE|  
|TableFulltextCatalogId|Table de charge de travail|**S'applique à**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].<br /><br /> ID du catalogue de texte intégral dans lequel résident les données d'indexation de texte intégral de la table.<br /><br /> Différent de zéro = ID de catalogue de texte intégral associé à l'index unique qui identifie les lignes dans une table indexée en texte intégral.<br /><br /> 0 = Table sans index de recherche en texte intégral.|  
|TableFulltextChangeTrackingOn|Table de charge de travail|**S'applique à**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].<br /><br /> Le suivi des modifications de texte intégral est activé pour la table.<br /><br /> 1 = TRUE<br /><br /> 0 = FALSE|  
|TableFulltextDocsProcessed|Table de charge de travail|**S'applique à**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].<br /><br /> Nombre de lignes traitées depuis le démarrage de l'indexation de texte intégral. Dans une table en cours d'indexation pour une recherche en texte intégral, toutes les colonnes d'une ligne sont considérées comme faisant partie d'un même document à indexer.<br /><br /> 0 = Aucune analyse ou indexation de texte intégral active n'est terminée.<br /><br /> > 0 = une des valeurs suivantes (A ou B) : A) Nombre de documents traités par des opérations d’insertion ou de mise à jour depuis le début d’une modification de remplissage intégral, incrémentiel ou manuel. B) Nombre de lignes traitées par des opérations d’insertion ou de mise à jour depuis l’activation des modifications avec le remplissage de l’index de mise à jour d’arrière-plan, depuis la modification du schéma d’index en texte intégral, depuis la reconstruction du catalogue de texte intégral, depuis que l’instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] a redémarré, etc.<br /><br /> NULL = La table n'a pas d'index de recherche en texte intégral.<br /><br /> Cette propriété ne contrôle pas et ne compte pas les lignes supprimées.|  
|TableFulltextFailCount|Table de charge de travail|**S'applique à**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].<br /><br /> Nombre de lignes que la recherche en texte intégral n'a pas indexées.<br /><br /> 0 = Le remplissage est terminé.<br /><br /> > 0 = une des valeurs suivantes (A ou B) : A) Nombre de documents qui n’ont pas été indexés depuis le début d’une mise à jour du remplissage intégral, incrémentiel ou manuel. B) Dans le cas d’un suivi des modifications avec index de mise à jour en arrière-plan, nombre de lignes qui n’ont pas été indexées depuis le début du remplissage, ou depuis le redémarrage du remplissage. Cela peut être causé par une modification du schéma, une reconstruction du catalogue, un redémarrage du serveur, etc.<br /><br /> NULL = La table n'a pas d'index de recherche en texte intégral.|  
|TableFulltextItemCount|Table de charge de travail|**S'applique à**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].<br /><br /> Nombre de lignes dont l'indexation de texte intégral a réussi.|  
|TableFulltextKeyColumn|Table de charge de travail|**S'applique à**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].<br /><br /> ID de la colonne associée à l'index de colonne unique qui fait partie de la définition de l'indexation de texte intégral.<br /><br /> 0 = Table sans index de recherche en texte intégral.|  
|TableFulltextPendingChanges|Table de charge de travail|**S'applique à**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].<br /><br /> Nombre d'entrées de suivi des modifications en attente de traitement.<br /><br /> 0 = Le suivi des modifications n'est pas activé.<br /><br /> NULL = La table n'a pas d'index de recherche en texte intégral.|  
|TableFulltextPopulateStatus|Table de charge de travail|**S'applique à**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].<br /><br /> 0 = Inactif.<br /><br /> 1 = Remplissage complet en cours.<br /><br /> 2 = Remplissage incrémentiel en cours.<br /><br /> 3 = Propagation des changements suivis en cours.<br /><br /> 4 = Création de l'index de mise à jour d'arrière-plan en cours, par exemple le suivi des modifications automatiques.<br /><br /> 5 = Indexation de texte intégral accélérée ou suspendue.|  
|TableHasActiveFulltextIndex|Table de charge de travail|**S'applique à**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].<br /><br /> La table dispose d'un index de recherche en texte intégral actif.<br /><br /> 1 = True<br /><br /> 0 = False|  
|TableHasCheckCnst|Table de charge de travail|La table comporte une contrainte CHECK.<br /><br /> 1 = True<br /><br /> 0 = False|  
|TableHasClustIndex|Table de charge de travail|La table comporte un index cluster.<br /><br /> 1 = True<br /><br /> 0 = False|  
|TableHasDefaultCnst|Table de charge de travail|La table comporte une contrainte DEFAULT.<br /><br /> 1 = True<br /><br /> 0 = False|  
|TableHasDeleteTrigger|Table de charge de travail|La table comporte un déclencheur DELETE.<br /><br /> 1 = True<br /><br /> 0 = False|  
|TableHasForeignKey|Table de charge de travail|La table comporte une contrainte FOREIGN KEY.<br /><br /> 1 = True<br /><br /> 0 = False|  
|TableHasForeignRef|Table de charge de travail|La table est référencée par une contrainte FOREIGN KEY.<br /><br /> 1 = True<br /><br /> 0 = False|  
|TableHasIdentity|Table de charge de travail|La table comporte une colonne d'identité.<br /><br /> 1 = True<br /><br /> 0 = False|  
|TableHasIndex|Table de charge de travail|La table comporte un index de type non défini.<br /><br /> 1 = True<br /><br /> 0 = False|  
|TableHasInsertTrigger|Table de charge de travail|L'objet comporte un déclencheur INSERT.<br /><br /> 1 = True<br /><br /> 0 = False|  
|TableHasNonclustIndex|Table de charge de travail|La table comporte un index non-cluster.<br /><br /> 1 = True<br /><br /> 0 = False|  
|TableHasPrimaryKey|Table de charge de travail|La table comporte une clé primaire.<br /><br /> 1 = True<br /><br /> 0 = False|  
|TableHasRowGuidCol|Table de charge de travail|La table comporte un ROWGUIDCOL pour une colonne **uniqueidentifier**.<br /><br /> 1 = True<br /><br /> 0 = False|  
|TableHasTextImage|Table de charge de travail|La table comporte une colonne **text**, **ntext** ou **image**.<br /><br /> 1 = True<br /><br /> 0 = False|  
|TableHasTimestamp|Table de charge de travail|La table comporte une colonne **timestamp**.<br /><br /> 1 = True<br /><br /> 0 = False|  
|TableHasUniqueCnst|Table de charge de travail|La table comporte une contrainte UNIQUE.<br /><br /> 1 = True<br /><br /> 0 = False|  
|TableHasUpdateTrigger|Table de charge de travail|L'objet comporte un déclencheur UPDATE.<br /><br /> 1 = True<br /><br /> 0 = False|  
|TableHasVarDecimalStorageFormat|Table de charge de travail|La table est activée pour le format de stockage **vardecimal**.<br /><br /> 1 = True<br /><br /> 0 = False|  
|TableInsertTrigger|Table de charge de travail|La table comporte un déclencheur INSERT.<br /><br /> >1 = ID du premier déclencheur du type spécifié.|  
|TableInsertTriggerCount|Table de charge de travail|La table comporte le nombre de déclencheurs INSERT spécifié.<br /><br /> >0 = Nombre de déclencheurs INSERT.|  
|TableIsFake|Table de charge de travail|La table n'est pas réelle. Elle est matérialisée en interne à la demande par le [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)].<br /><br /> 1 = True<br /><br /> 0 = False|  
|TableIsLockedOnBulkLoad|Table de charge de travail|La table est verrouillée à cause d’une tâche **bcp** ou BULK INSERT.<br /><br /> 1 = True<br /><br /> 0 = False|  
|TableIsMemoryOptimized|Table de charge de travail|**S'applique à**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].<br /><br /> La table est optimisée en mémoire<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> Type de données de base : **int**<br /><br /> Pour plus d’informations, consultez [OLTP en mémoire &#40;optimisation en mémoire&#41;](../../relational-databases/in-memory-oltp/in-memory-oltp-in-memory-optimization.md).|  
|TableIsPinned|Table de charge de travail|La table est épinglée pour être conservée dans le cache de données.<br /><br /> 0 = False<br /><br /> Cette fonctionnalité n'est pas prise en charge par [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] et versions ultérieures|  
|TableTextInRowLimit|Table de charge de travail|Nombre maximal d'octets autorisé pour text in row.<br /><br /> 0 si l'option text in row n'est pas définie.|  
|TableUpdateTrigger|Table de charge de travail|La table comporte un déclencheur UPDATE.<br /><br /> > 1 = ID du premier déclencheur du type spécifié.|  
|TableUpdateTriggerCount|Table de charge de travail|La table comporte le nombre de déclencheurs UPDATE spécifié.<br /><br /> > 0 = Nombre de déclencheurs UPDATE.|  
|TableHasColumnSet|Table de charge de travail|La table comporte un jeu de colonnes.<br /><br /> 0 = False<br /><br /> 1 = True<br /><br /> Pour plus d’informations, consultez [Utiliser des jeux de colonnes](../../relational-databases/tables/use-column-sets.md).|  
|TableTemporalType|Table de charge de travail|**S'applique à**: [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].<br /><br /> Spécifie le type de table.<br /><br /> 0 = table non temporelle<br /><br /> 1 = table historique de la table à système par version<br /><br /> 2 = table temporelle à système par version|  
  
## <a name="return-types"></a>Types de retour  
 **Int**  
  
## <a name="exceptions"></a>Exceptions  
 Retourne la valeur NULL en cas d'erreur ou si un appelant n'est pas autorisé à afficher l'objet.  
  
 Un utilisateur peut voir uniquement les métadonnées des éléments sécurisables qui lui appartiennent ou pour lesquels il dispose d'une autorisation. Cela signifie que les fonctions intégrées générant des métadonnées, telles que OBJECTPROPERTY, peuvent retourner la valeur NULL si l'utilisateur ne dispose d'aucune autorisation sur l'objet. Pour plus d'informations, consultez [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="remarks"></a>Notes   
 [!INCLUDE[ssDE](../../includes/ssde-md.md)] considère que *object_id* se situe dans le contexte de la base de données active. Une requête référençant un *object_id* dans une autre base de données retourne NULL ou des résultats incorrects. Par exemple, dans la requête qui suit, le contexte de la base de données active est la base de données MASTER. Le [!INCLUDE[ssDE](../../includes/ssde-md.md)] tentera de renvoyer la valeur de la propriété de l’argument *object_id* spécifié dans cette base de données et non dans la base de données spécifiée dans la requête. La requête retourne des résultats incorrects car la vue `vEmployee` ne se trouve pas dans la base de données MASTER.  
  
```  
USE master;  
GO  
SELECT OBJECTPROPERTY(OBJECT_ID(N'AdventureWorks2012.HumanResources.vEmployee'), 'IsView');  
GO  
```  
  
 OBJECTPROPERTY(*view_id*, 'IsIndexable') peut utiliser un volume important de ressources système, car l’évaluation de la propriété IsIndexable nécessite l’analyse de la définition de la vue, sa normalisation et son optimisation partielle. Bien que la propriété IsIndexable identifie les tables ou les vues qui peuvent être indexées, la création réelle de l'index peut malgré tout échouer si certaines conditions de clé d'index ne sont pas remplies. Pour plus d’informations, consultez [CREATE INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-index-transact-sql.md).  
  
 OBJECTPROPERTY(*table_id*, 'TableHasActiveFulltextIndex') retourne la valeur 1 (true) lorsqu’au moins une colonne d’une table est ajoutée pour l’indexation. L'indexation de texte intégral est activée au niveau du remplissage dès l'ajout de la première colonne à indexer.  
  
 Lors de la création d'une table, l'option QUOTED IDENTIFIER est toujours stockée avec la valeur ON dans les métadonnées de la table, même si elle a la valeur OFF au moment de sa création. Par conséquent, OBJECTPROPERTY(*table_id*, 'IsQuotedIdentOn’) retourne toujours la valeur 1 (true).  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-verifying-that-an-object-is-a-table"></a>A. Vérification qu'un objet est une table  
 L'exemple suivant teste si `UnitMeasure` est une table dans la base de données [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)].  
  
```  
USE AdventureWorks2012;  
GO  
IF OBJECTPROPERTY (OBJECT_ID(N'Production.UnitMeasure'),'ISTABLE') = 1  
   PRINT 'UnitMeasure is a table.'  
ELSE IF OBJECTPROPERTY (OBJECT_ID(N'Production.UnitMeasure'),'ISTABLE') = 0  
   PRINT 'UnitMeasure is not a table.'  
ELSE IF OBJECTPROPERTY (OBJECT_ID(N'Production.UnitMeasure'),'ISTABLE') IS NULL  
   PRINT 'ERROR: UnitMeasure is not a valid object.';  
GO  
  
```  
  
### <a name="b-verifying-that-a-scalar-valued-user-defined-function-is-deterministic"></a>B. Vérification du déterminisme d'une fonction définie par l'utilisateur à valeur scalaire  
 L’exemple suivant teste si la fonction `ufnGetProductDealerPrice` (à valeur scalaire et définie par l’utilisateur), qui retourne une valeur **money**, est déterministe.  
  
```  
USE AdventureWorks2012;  
GO  
SELECT OBJECTPROPERTY(OBJECT_ID('dbo.ufnGetProductDealerPrice'), 'IsDeterministic');  
GO  
```  
  
 Le jeu de résultats révèle que `ufnGetProductDealerPrice` n'est pas une fonction déterministe.  
  
 ```
-----  
0
```  
  
### <a name="c-finding-the-tables-that-belong-to-a-specific-schema"></a>C. Recherche des tables appartenant à un schéma spécifique  
 L’exemple suivant retourne toutes les tables figurant dans le schéma dbo.  
  
```  
-- Uses AdventureWorks  
  
SELECT name, object_id, type_desc  
FROM sys.objects   
WHERE OBJECTPROPERTY(object_id, N'SchemaId') = SCHEMA_ID(N'dbo')  
ORDER BY type_desc, name;  
GO  
```  
  
## <a name="examples-includesssdwfullincludessssdwfull-mdmd-and-includesspdwincludessspdw-mdmd"></a>Exemples : [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] et [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="d-verifying-that-an-object-is-a-table"></a>D. Vérification qu’un objet est une table  
 L'exemple suivant teste si `dbo.DimReseller` est une table dans la base de données [!INCLUDE[ssawPDW](../../includes/ssawpdw-md.md)].  
  
```  
-- Uses AdventureWorks  
  
IF OBJECTPROPERTY (OBJECT_ID(N'dbo.DimReseller'),'ISTABLE') = 1  
   SELECT 'DimReseller is a table.'  
ELSE   
   SELECT 'DimReseller is not a table.';  
GO  
```  
  
## <a name="see-also"></a> Voir aussi  
 [COLUMNPROPERTY &#40;Transact-SQL&#41;](../../t-sql/functions/columnproperty-transact-sql.md)   
 [Fonctions de métadonnées &#40;Transact-SQL&#41;](../../t-sql/functions/metadata-functions-transact-sql.md)   
 [OBJECTPROPERTYEX &#40;Transact-SQL&#41;](../../t-sql/functions/objectpropertyex-transact-sql.md)   
 [ALTER AUTHORIZATION &#40;Transact-SQL&#41;](../../t-sql/statements/alter-authorization-transact-sql.md)   
 [TYPEPROPERTY &#40;Transact-SQL&#41;](../../t-sql/functions/typeproperty-transact-sql.md)   
 [sys.objects &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-objects-transact-sql.md)  
  
  

