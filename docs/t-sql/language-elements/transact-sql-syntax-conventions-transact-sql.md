---
title: Conventions de la syntaxe Transact-SQL (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/09/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.component: t-sql|language-elements
ms.reviewer: ''
ms.suite: sql
ms.technology: t-sql
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- sql13.TSQLExpandPortal.f1
dev_langs:
- TSQL
helpviewer_keywords:
- conventions [SQL Server]
- Applies to section in Transact-SQL topics
- code example conventions [SQL Server]
- objects [SQL Server], names
- code [SQL Server], conventions
- multipart names [SQL Server]
- Transact-SQL syntax conventions
- syntax conventions [SQL Server]
- code [SQL Server]
- Transact-SQL
- naming conventions [SQL Server]
- syntax [SQL Server], Transact-SQL
ms.assetid: 35fbcf7f-8b55-46cd-a957-9b8c7b311241
caps.latest.revision: 55
author: douglaslMS
ms.author: douglasl
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: 3339a53c2569f6561caa4cefdb5e697610c71fdd
ms.sourcegitcommit: 808d23a654ef03ea16db1aa23edab496b73e5072
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/04/2018
ms.locfileid: "34582111"
---
# <a name="transact-sql-syntax-conventions-transact-sql"></a>Conventions de la syntaxe Transact-SQL (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-all_md](../../includes/tsql-appliesto-ss2008-all-md.md)]

  Le tableau suivant répertorie et décrit les conventions utilisées dans les diagrammes de syntaxe du Guide de référence [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
|Convention|Utilisé pour|  
|----------------|--------------|  
|MAJUSCULES|Mots clés [!INCLUDE[tsql](../../includes/tsql-md.md)].|  
|*italique*|Paramètres de syntaxe [!INCLUDE[tsql](../../includes/tsql-md.md)] fournis par l'utilisateur.|  
|**gras**|Noms de bases de données, de tables, de colonnes, d'index, de procédures stockées, d'utilitaires, de types de données et de textes à taper tel qu'indiqués.|  
|_underline_|Indique que l'instruction est omise par la valeur par défaut appliquée lorsque la clause contient la valeur soulignée.|  
|&#124; (barre verticale)|Sépare les éléments de syntaxe placés entre crochets ou entre accolades. Vous ne pouvez utiliser qu'un seul de ces éléments.|  
|`[ ]` (crochets)|Éléments de syntaxe facultatifs. Ne tapez pas les crochets.|  
|{} (accolades)|Éléments de syntaxe obligatoires. Ne tapez pas les accolades.|  
|[**,**...*n*]|Indique que l’élément précédent peut se répéter *n* fois. Les occurrences sont séparées par des virgules.|  
|[...*n*]|Indique que l’élément précédent peut se répéter *n* fois. Les occurrences sont séparées par des espaces.|  
|;|Terminateur d'instruction [!INCLUDE[tsql](../../includes/tsql-md.md)]. Bien que le point-virgule ne soit pas requis pour la plupart des instructions dans cette version de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], il sera requis dans une version à venir.|  
|\<étiquette> ::=|Nom d'un bloc de syntaxe. Cette convention est utilisée pour regrouper et étiqueter des sections de syntaxe longue ou une unité de syntaxe pouvant apparaître à plusieurs emplacements au sein d'une instruction. Tous les emplacements dans lesquels le bloc de syntaxe peut être utilisé sont signalés par une étiquette encadrée de chevrons : \<étiquette>.<br /><br /> Un jeu est une collection d’expressions, par exemple un \<jeu de regroupement>, et une liste est une collection de jeux, par exemple une \<liste d’éléments composites>.|  
  
## <a name="multipart-names"></a>Noms à plusieurs composantes  
 Sauf indication contraire, toutes les références [!INCLUDE[tsql](../../includes/tsql-md.md)] au nom d'un objet de base de données peuvent prendre la forme d'un nom à quatre composantes, comme suit :  
  
*server_name* **.**[*database_name*]**.**[*schema_name*]**.***object_name*  
  
 | *database_name***.**[* schema_name *]**.***object_name*  
  
 | *schema_name ***.*** object_name*  
  
 | *object_name*  
  
*server_name*  
Spécifie un nom de serveur lié ou un nom de serveur distant.  
  
*database_name*  
Spécifie le nom d'une base de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] lorsque l'objet réside dans une instance locale de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Quand l’objet se trouve dans un serveur lié, *database_name* spécifie un catalogue OLE DB.  
  
*schema_name*  
Spécifie le nom du schéma contenant l'objet si celui-ci réside dans une base de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Lorsque l’objet se trouve dans un serveur lié, *schema_name* spécifie un nom de schéma OLE DB.  
  
*object_name*  
Fait référence au nom de l'objet.  
  
Lorsque vous faites référence à un objet en particulier, il n'est pas toujours nécessaire de spécifier le serveur, la base de données et le schéma pour que le [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] l'identifie. Cependant, si l’objet est introuvable, un message d’erreur est retourné.  
  
> [!NOTE]  
> Pour éviter des erreurs relatives à la résolution du nom, nous vous recommandons de spécifier le nom du schéma chaque fois que vous indiquez un objet correspondant.  
  
Pour omettre des nœuds intermédiaires, utilisez des points pour indiquer ces emplacements. Le tableau suivant répertorie les formats valides des noms d'objets.  
  
|Format de la référence d'objet|Description|  
|-----------------------------|-----------------|  
|*server* **.** *database* **.** *schema* **.** *object*|Nom à quatre composantes.|  
|*server* **.** *database* **..** *object*|Le nom du schéma est omis.|  
|*server* **..** *schema* **.** *object*|Le nom de la base de données est omis.|  
|*server* **...** *object*|Les noms de la base de données et du schéma sont omis.|  
|*database* **.** *schema* **.** *object*|Le nom du serveur est omis.|  
|*database* **..** *object*|Les noms du serveur et du schéma sont omis.|  
|*schema* **.** *object*|Les noms du serveur et de la base de données sont omis.|  
|*object*|Les noms du serveur, de la base de données et du schéma sont omis.|  
  
## <a name="code-example-conventions"></a>Conventions des exemples de code  
Sauf indication contraire, les exemples fournis dans le Guide de référence [!INCLUDE[tsql](../../includes/tsql-md.md)] ont été testés à l'aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] et de ses paramètres par défaut pour les options suivantes :  
  
-   ANSI_NULLS  
-   ANSI_NULL_DFLT_ON  
-   ANSI_PADDING  
-   ANSI_WARNINGS  
-   CONCAT_NULL_YIELDS_NULL  
-   QUOTED_IDENTIFIER  
  
La plupart des exemples de code figurant dans le Guide de référence [!INCLUDE[tsql](../../includes/tsql-md.md)] ont été testés sur des serveurs prenant en charge un ordre de tri respectant la casse. Généralement, les serveurs de test ont exécuté la page de codes ANSI/ISO 1252.  
  
De nombreux exemples de code font précéder les constantes de chaînes de caractères Unicode de la lettre **N**. Sans le préfixe **N**, la chaîne est convertie en page de codes par défaut de la base de données. Cette page risque de ne pas reconnaître certains caractères.  
  
## <a name="applies-to-references"></a>Informations de référence de type « S'applique à »  
Les informations de référence sur [!INCLUDE[tsql](../../includes/tsql-md.md)] comprennent des articles sur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (entre [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]), [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] et [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)].   

En haut de chaque article se trouve une section indiquant les produits qui prennent en charge le sujet de l’article. Si un produit est omis, alors la fonctionnalité décrite par l’article n’est pas disponible dans ce produit. Par exemple, les groupes de disponibilité ont été introduits dans [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]. L’article **CREATE AVAILABILITY GROUP** indique qu’il s’applique à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (entre [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] et [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]), car il ne s’applique pas à [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)], [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] et [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].  
  
Dans certains cas, le sujet général de l’article peut être utilisé dans un produit, mais tous les arguments ne sont pas pris en charge. Par exemple, les utilisateurs de base de données à relation contenant-contenu ont été introduits dans [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]. L’instruction **CREATE USER** peut être utilisée dans tous les produits [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], mais la syntaxe **WITH PASSWORD** ne peut pas être utilisée avec les versions plus anciennes. Dans ce cas, des sections **S’applique à** supplémentaires sont insérées dans les descriptions des arguments appropriés dans le corps de l’article.  
  
## <a name="see-also"></a> Voir aussi  
[Référence Transact-SQL &#40;moteur de base de données&#41;](../../t-sql/transact-sql-reference-database-engine.md)    
[Mots clés réservés &#40;Transact-SQL&#41;](../../t-sql/language-elements/reserved-keywords-transact-sql.md)      
[Problèmes de conception de Transact-SQL](http://msdn.microsoft.com/library/dd193411.aspx)    
[Problèmes de nommage de Transact-SQL](http://msdn.microsoft.com/library/dd193246.aspx)        
[Problèmes de performances de Transact-SQL](http://msdn.microsoft.com/library/dd172117.aspx)    


