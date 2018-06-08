---
title: Configuration de la visibilité des métadonnées | Microsoft Docs
ms.custom: ''
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.component: security
ms.reviewer: ''
ms.suite: sql
ms.technology:
- database-engine
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- subcomponents visibility [SQL Server]
- metadata [SQL Server], visibility
- permissions [SQL Server], metadata access
- viewing metadata
- objects [SQL Server], metadata
- displaying metadata
- database metadata [SQL Server]
- metadata [SQL Server], permissions
ms.assetid: 50d2e015-05ae-4014-a1cd-4de7866ad651
caps.latest.revision: 51
author: edmacauley
ms.author: edmaca
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: 8b0d06ef7357d4986cc9af5902d36939efaadf24
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="metadata-visibility-configuration"></a>Configuration de la visibilité des métadonnées
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
  La visibilité des métadonnées est limitée aux éléments sécurisables qu'un utilisateur détient ou pour lesquels des autorisations lui ont été accordées. Par exemple, la requête suivante retourne une ligne si l'utilisateur bénéficie d'une autorisation telle que SELECT ou INSERT sur la table `myTable`.  
  
```  
SELECT name, object_id  
FROM sys.tables  
WHERE name = 'myTable';  
GO  
```  
  
 Toutefois, si l'utilisateur ne dispose pas d'autorisation sur `myTable`, la requête renvoie un jeu de résultats vide.  
  
## <a name="scope-and-impact-of-metadata-visibility-configuration"></a>Portée et impact de la configuration de la visibilité des métadonnées  
 La configuration de la visibilité des métadonnées ne s'applique qu'aux éléments sécurisables ci-dessous.  
  
|||  
|-|-|  
|Affichages catalogue|Procédures stockées du [!INCLUDE[ssDE](../../includes/ssde-md.md)] **sp_help**|  
|Métadonnées exposant des fonctions intégrées|Affichages des schémas d'information|  
|vues de compatibilité ;|Propriétés étendues|  
  
 La configuration de la visibilité des métadonnées ne s'applique pas aux éléments sécurisables ci-dessous.  
  
|||  
|-|-|  
|Tables système de copie des journaux de transaction|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Tables système de l’Agent|  
|Tables système des plans de maintenance de base de données|Tables système de sauvegardes|  
|Tables système de réplication|Procédures stockées [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sp_help **de la réplication et de l’Agent**|  
  
 Une accessibilité restreinte aux métadonnées implique les conséquences suivantes :  
  
-   Les applications qui sous-entendent un accès aux métadonnées **public** s’interrompront.  
  
-   Les requêtes portant sur les vues système risqueront de renvoyer simplement un sous-ensemble de lignes et même parfois un jeu de résultats vide.  
  
-   Les fonctions intégrées émettant des métadonnées telles que OBJECTPROPERTYEX peuvent renvoyer une valeur NULL.  
  
-   Les procédures stockées [!INCLUDE[ssDE](../../includes/ssde-md.md)] **sp_help** peuvent retourner uniquement un sous-ensemble de lignes ou la valeur NULL.  
  
 Les modules SQL, tels que les procédures stockées et les déclencheurs, fonctionnent dans le contexte de sécurité de l'appelant et, par conséquent, auront un accès limité aux métadonnées. Dans le code suivant par exemple, lorsque la procédure stockée tente d'accéder aux métadonnées de la table `myTable` pour laquelle l'appelant n'a aucune autorisation, un jeu de résultats vide est renvoyé. Dans les versions antérieures de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], une ligne est renvoyée.  
  
```  
CREATE PROCEDURE assumes_caller_can_access_metadata  
BEGIN  
SELECT name, id   
FROM sysobjects   
WHERE name = 'myTable';  
END;  
GO  
```  
  
 Si vous voulez autoriser les appelants à consulter les métadonnées, vous pouvez leur accorder l'autorisation VIEW DEFINITION avec une étendue appropriée : niveau de l'objet, niveau de la base de données ou niveau du serveur. Si, dans le cas de l'exemple précédent, l'appelant bénéficie d'une autorisation VIEW DEFINITION sur `myTable`, la procédure stockée renvoie une ligne. Pour plus d’informations, consultez [GRANT &#40;Transact-SQL&#41;](../../t-sql/statements/grant-transact-sql.md) et [GRANT – octroi d’autorisations de base de données &#40;Transact-SQL&#41;](../../t-sql/statements/grant-database-permissions-transact-sql.md).  
  
 Vous pouvez également modifier la procédure stockée pour qu'elle s'exécute avec les informations d'identification du propriétaire. Si le propriétaire de la procédure et celui de la table sont identiques, le chaînage des propriétés s'applique et le contexte de sécurité du propriétaire de la procédure permet d'accéder aux métadonnées de `myTable`. Dans ces circonstances, le code suivant renvoie une ligne de métadonnées à l'appelant.  
  
> [!NOTE]  
>  L’exemple suivant utilise l’affichage catalogue [sys.objects](../../relational-databases/system-catalog-views/sys-objects-transact-sql.md) au lieu de l’affichage de compatibilité [sys.sysobjects](../../relational-databases/system-compatibility-views/sys-sysobjects-transact-sql.md) .  
  
```  
CREATE PROCEDURE does_not_assume_caller_can_access_metadata  
WITH EXECUTE AS OWNER  
AS  
BEGIN  
SELECT name, id  
FROM sys.objects   
WHERE name = 'myTable'   
END;  
GO  
```  
  
> [!NOTE]  
>  Vous pouvez utiliser EXECUTE AS pour basculer temporairement dans le contexte de sécurité de l'appelant. Pour plus d’informations, consultez [EXECUTE AS &#40;Transact-SQL&#41;](../../t-sql/statements/execute-as-transact-sql.md).  
  
## <a name="benefits-and-limits-of-metadata-visibility-configuration"></a>Avantages et inconvénients de la configuration de la visibilité des métadonnées  
 La configuration de la visibilité des métadonnées peut jouer un rôle important dans votre plan de sécurité global. Sachez quand même qu'il existe des cas où un utilisateur expérimenté et déterminé peut enfreindre la confidentialité de certaines métadonnées. Nous vous recommandons de déployer des autorisations d'accès aux métadonnées comme d'autres mesures préventives efficaces.  
  
 Il est théoriquement possible d'imposer l'émission de métadonnées dans les messages d'erreur en manipulant l'ordre d'évaluation de prédicat dans les requêtes. L’éventualité d’ *attaques de type essai et erreur* de cette sorte n’est pas propre à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Elle provient implicitement des transformations associatives et commutatives autorisées en algèbre relationnel. Vous pouvez réduire ce risque en limitant les informations retournées dans les messages d'erreur. Pour restreindre encore la visibilité des métadonnées dans ce sens, vous pouvez démarrer le serveur avec l'indicateur de trace 3625. Cet indicateur de trace limite la quantité d'informations affichées dans les messages d'erreur. À son tour, cela contribue à éviter les divulgations forcées. En échange, les messages d'erreur seront succincts et plus difficiles à utiliser à des fins de débogage. Pour plus d’informations, consultez [Options de démarrage du service moteur de base de données](../../database-engine/configure-windows/database-engine-service-startup-options.md) et [Indicateurs de trace &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md).  
  
 Les métadonnées suivantes ne peuvent pas être violées :  
  
-   Valeur stockée dans la colonne **provider_string** de **sys.servers**. Sans autorisation ALTER ANY LINKED SERVER, un utilisateur verra une valeur NULL dans cette colonne.  
  
-   Définition source d'un objet défini par l'utilisateur tel qu'une procédure stockée ou un déclencheur. Le code source n'est visible que si l'une des conditions suivantes est remplie :  
  
    -   L'utilisateur dispose de l'autorisation VIEW DEFINITION pour l'objet.  
  
    -   L'autorisation VIEW DEFINITION pour l'objet n'a pas été refusée à l'utilisateur et ce dernier bénéficie de l'autorisation CONTROL, ALTER ou TAKE OWNERSHIP sur l'objet. Tous les autres utilisateurs verront la valeur NULL.  
  
-   Colonnes de définitions qui se trouvent dans les affichages catalogue suivants :  
  
    |||  
    |-|-|  
    |**sys.all_sql_modules**|**sys.sql_modules**|  
    |**sys.server_sql_modules**|**sys.check_constraints**|  
    |**sys.default_constraints**|**sys.computed_columns**|  
    |**sys.numbered_procedures**||  
  
-   Colonne **ctext** de l’affichage de compatibilité **syscomments**  
  
-   Sortie de la procédure stockée **sp_helptext**  
  
-   Colonnes suivantes des affichages des schémas d'information :  
  
    |||  
    |-|-|  
    |INFORMATION_SCHEMA.CHECK_CONSTRAINTS.CHECK_CLAUSE|INFORMATION_SCHEMA.COLUMNS.COLUMN_DEFAULT|  
    |INFORMATION_SCHEMA.DOMAINS.DOMAIN_DEFAULT|INFORMATION_SCHEMA.ROUTINE_COLUMNS.COLUMN_DEFAULT|  
    |INFORMATION_SCHEMA.ROUTINES.ROUTINE_DEFINITION|INFORMATION_SCHEMA.VIEWS.VIEW_DEFINITION|  
  
-   Fonction OBJECT_DEFINITION()  
  
-   Valeur stockée dans la colonne password_hash de **sys.sql_logins**.  Un utilisateur qui ne possède pas l'autorisation CONTROL SERVER verra une valeur NULL dans cette colonne.  
  
> [!NOTE]  
>  Les définitions SQL des procédures et des fonctions système intégrées sont visibles par tous par l’intermédiaire de l’affichage catalogue **sys.system_sql_modules** , de la procédure stockée **sp_helptext** et de la fonction OBJECT_DEFINITION().  
  
## <a name="general-principles-of-metadata-visibility"></a>Principes généraux de la visibilité des métadonnées  
 Voici quelques points à prendre en compte en termes de visibilité des métadonnées :  
  
-   Autorisations implicites des rôles fixes  
  
-   Étendue des autorisations  
  
-   Priorité de DENY  
  
-   Visibilité des métadonnées des sous-composants  
  
### <a name="fixed-roles-and-implicit-permissions"></a>Rôles fixes et autorisations implicites  
 Les métadonnées accessibles par les rôles fixes dépendent des autorisations implicites qui s'y rapportent.  
  
### <a name="scope-of-permissions"></a>Étendue des autorisations  
 Les autorisations délivrées à une étendue impliquent la possibilité de voir les métadonnées à ce niveau et à tous les niveaux inférieurs qui en dépendent. Par exemple, si une autorisation SELECT a été accordée à un utilisateur sur un schéma, le bénéficiaire possède l'autorisation SELECT sur tous les éléments sécurisables contenus dans ce schéma. L'octroi de l'autorisation SELECT sur un schéma permet donc à un utilisateur de consulter les métadonnées du schéma ainsi que des tables, vues, fonctions, procédures, files d'attente, synonymes, types et collections de schémas XML qu'il renferme. Pour plus d’informations sur les étendues, consultez [Hiérarchie des autorisations &#40;moteur de base de données&#41;](../../relational-databases/security/permissions-hierarchy-database-engine.md).  
  
### <a name="precedence-of-deny"></a>Priorité de DENY  
 DENY a généralement la priorité sur toutes les autres autorisations. Par exemple, si un utilisateur de base de données dispose d'une autorisation EXECUTE sur un schéma, mais que l'autorisation EXECUTE lui a été refusée sur une procédure stockée de ce schéma, l'utilisateur ne peut pas voir les métadonnées de cette procédure stockée.  
  
 De plus, si un utilisateur se voit refuser l'autorisation EXECUTE pour un schéma alors qu'il dispose de l'autorisation EXECUTE pour une procédure stockée de ce schéma, l'utilisateur ne peut pas voir les métadonnées de cette procédure stockée.  
  
 Prenons un autre exemple. Si l'autorisation EXECUTE a été accordée, puis refusée à un utilisateur sur une procédure stockée, ce qui est possible par le biais de diverses appartenances aux rôles, DENY reste prioritaire et l'utilisateur ne peut pas voir les métadonnées de la procédure stockée.  
  
### <a name="visibility-of-subcomponent-metadata"></a>Visibilité des métadonnées des sous-composants  
 La visibilité des sous-composants que sont les index, les contraintes de validation et les déclencheurs est déterminée par les autorisations accordées au parent. Ces sous-composants ne bénéficient pas d'autorisations transmissibles. Par exemple, si un utilisateur dispose d'une autorisation sur une table, il peut voir les métadonnées correspondant aux colonnes de la table, aux index, aux contraintes de validation, aux déclencheurs et à d'autres sous-composants de la sorte.  
  
#### <a name="metadata-that-is-accessible-to-all-database-users"></a>Métadonnées accessibles à tous les utilisateurs de la base de données  
 Certaines métadonnées doivent rester accessibles à tous les utilisateurs dans une base de données spécifique. Par exemple, dans la mesure où les groupes de fichiers ne peuvent pas recevoir d'autorisations, il est impossible d'octroyer à un utilisateur l'autorisation de consulter les métadonnées d'un groupe de fichiers. En revanche, tout utilisateur qui peut créer une table doit pouvoir accéder aux métadonnées du groupe de fichiers pour utiliser les clauses ON *groupe_fichiers* ou TEXTIMAGE_ON *groupe_fichiers* de l’instruction CREATE TABLE.  
  
 Les métadonnées renvoyées par les fonctions DB_ID() et DB_NAME() sont visibles par tous les utilisateurs.  
  
 Le tableau ci-dessous répertorie les affichages catalogue qui sont visibles par le rôle **public** .  
  
|||  
|-|-|  
|**sys.partition_functions**|**sys.partition_range_values**|  
|**sys.partition_schemes**|**sys.data_spaces**|  
|**sys.filegroups**|**sys.destination_data_spaces**|  
|**sys.database_files**|**sys.allocation_units**|  
|**sys.partitions**|**sys.messages**|  
|**sys.schemas**|**sys.configurations**|  
|**sys.sql_dependencies**|**sys.type_assembly_usages**|  
|**sys.parameter_type_usages**|**sys.column_type_usages**|  
  
## <a name="see-also"></a> Voir aussi  
 [GRANT &#40;Transact-SQL&#41;](../../t-sql/statements/grant-transact-sql.md)   
 [DENY &#40;Transact-SQL&#41;](../../t-sql/statements/deny-transact-sql.md)   
 [REVOKE &#40;Transact-SQL&#41;](../../t-sql/statements/revoke-transact-sql.md)   
 [Clause EXECUTE AS &#40;Transact-SQL&#41;](../../t-sql/statements/execute-as-clause-transact-sql.md)   
 [Affichages catalogue &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [Affichages de compatibilité &#40;Transact-SQL&#41;](~/relational-databases/system-compatibility-views/system-compatibility-views-transact-sql.md)  
  
  
