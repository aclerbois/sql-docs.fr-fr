---
title: Les fonctions d’accès incompatibles (AccessToSQL) | Documents Microsoft
ms.prod: sql
ms.prod_service: sql-tools
ms.component: ssma-access
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
helpviewer_keywords:
- Access databases
- Access databases, features incompatible with SQL Azure
- Access databases, features incompatible with SQL Server
- dates
- default expressions
- foreign keys
- hyperlink columns
- incompatible Access features
- indexes
- indexes, length of
- keywords
- primary keys
- reference, incompatible features
- replication columns
- special characters
- unique indexes
- validation rules
ms.assetid: 99d45b9c-e3b9-4d56-8c25-b594b887ace1
caps.latest.revision: 18
author: Shamikg
ms.author: Shamikg
manager: craigg
ms.openlocfilehash: dbfcb395194371bf761f340cc74943791a649aaa
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="incompatible-access-features-accesstosql"></a>Fonctions d’accès incompatibles (AccessToSQL)
Pas toutes les fonctionnalités de base de données Access sont compatibles avec [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)]. Par exemple, [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] et accès contiennent différents ensembles de mots clés réservés. Problèmes tels que ceux-ci peuvent empêcher la migration vers [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)]. Utilisez le tableau suivant pour en savoir plus sur les problèmes de migration possibles et ce que vous pouvez faire à leur sujet.  
  
## <a name="database-settings-or-features-that-might-affect-migration"></a>Paramètres de base de données ou fonctionnalités susceptibles d’affecter la Migration  
  
|Accéder à la base de données ou d’une fonctionnalité|Problème de migration|  
|--------------------------------------|-------------------|  
|Accéder aux tables n’ont pas d’index uniques.|Si une table qui n’a pas un index unique est migrée vers [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)], vous ne pouvez pas modifier la table après la migration. Cela peut entraîner des problèmes de compatibilité des applications.<br /><br />Lorsque vous convertissez des objets de base de données Access, la fenêtre Sortie répertorie toutes les tables d’accès qui n’ont pas d’index uniques.<br /><br />Vous pouvez configurer l’accès afin d’ajouter une clé primaire sur la [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] table lors de la conversion. Pour plus d’informations, consultez [paramètres du projet (Conversion)](http://msdn.microsoft.com/en-us/bcebc635-c638-4ddb-924c-b9ccfef86388).|  
|Accéder aux tables ont des colonnes de la réplication.|Si une table Access qui inclut les colonnes système de réplication est migrée vers [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)], la fonctionnalité de réplication Jet sera rompue après la migration.<br /><br />Après la migration, envisagez d’utiliser [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] la réplication pour mettre à jour les copies synchronisées de vos bases de données.|  
|Accéder aux tables qui ont des index uniques contiennent plusieurs valeurs null.|Accéder aux tables qui ont un index unique avec plusieurs valeurs null ne peut pas être transféré à [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)], car dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)], les index uniques ne pas autoriser plusieurs valeurs NULL. La migration échoue pour ces tables.<br /><br />SSMA marquera ce problème dans les rapports d’évaluation. Pour créer un rapport d’évaluation, consultez [évaluation des objets de base de données Access pour la Conversion](http://msdn.microsoft.com/en-us/8b9e23d6-da62-437a-8c05-8ad2628b9441).<br /><br />Si ce problème se produit, il se peut que vous devez vous assurer que la clé primaire n’a pas de valeurs null en double. Ou bien, vous devez supprimer la clé primaire ou un index uniques qui contiennent plusieurs valeurs null.|  
|Accéder aux tables contiennent des valeurs de date qui sont hors de la [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] plage.|Le [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] **datetime** type accepte les dates dans la plage de 1 janvier 1753 au 31 décembre 9999 uniquement. Accès accepte les dates dans la plage de 1 100 janvier au 31 décembre 9999.<br /><br />SSMA marquera ce problème dans les rapports d’évaluation. Pour créer un rapport d’évaluation, consultez [évaluation des objets de base de données Access pour la Conversion](http://msdn.microsoft.com/en-us/8b9e23d6-da62-437a-8c05-8ad2628b9441).<br /><br />Vous pouvez configurer comment SSMA résout les dates qui sont hors de la [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] plage. Pour plus d’informations, consultez [paramètres du projet (Migration)](http://msdn.microsoft.com/en-us/4caebc9c-8680-4b99-a8fa-89c43161c95d).|  
|Longueurs des index dans Access dépassent 900 octets.|[!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] les index ont une limite de 900 octets pour la taille totale des colonnes clés d’index. Si vos tables Access utilisent des index plus grands, SSMA affiche un avertissement.<br /><br />Si vous poursuivez la migration des données, la migration risque d’échouer.|  
|Accès aux noms d’objet sont [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] mots clés, ou contenir des caractères spéciaux.|Accès et [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] ont différents ensembles de mots clés réservés et caractères spéciaux. [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] accepte les objets qui sont nommées à l’aide de [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] mots clés ou qui contiennent des caractères spéciaux si vous utilisez des identificateurs entre crochets ou entre guillemets, par exemple, « select » ou [] .p. Pour plus d’informations, consultez « Identificateurs délimité (moteur de base de données) » dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] la documentation en ligne.<br /><br />**Remarque :** pour utiliser des guillemets pour délimiter les identificateurs, SET QUOTED_IDENTIFIER doit avoir la valeur ON.<br /><br />Par exemple, `CREATE TABLE [schema](c1 [FOR])` est une instruction valide, même si **schéma** et **pour** sont des mots clés réservés. En outre, `CREATE TABLE [xxx*yyy](c1 x&y)` est une instruction valide, même si le nom de table et de colonne contenir les caractères spéciaux  **\&#42 ;** et **&amp;**.<br /><br />Toutes les requêtes qui font référence à ces objets doivent également utiliser les noms avec crochets ou guillemets. Par exemple, la requête `SELECT * FROM schema` échoue. La requête correcte est : `SELECT * FROM [schema]`.<br /><br />Lorsque vous convertissez des objets de base de données Access, le volet de résultats répertorie toutes les tables Access qui utilisent des mots clés ou des caractères spéciaux. Vous pouvez modifier les tables dans Access, puis supprimez et rajoutez la base de données ; ou bien, vous pouvez modifier les requêtes qui référencent ces objets afin que les requêtes utilisent des crochets ou des guillemets pour délimiter des identificateurs. Si vous ne modifiez pas vos requêtes, vos applications Access peuvent renvoyer des erreurs ou d’autres problèmes.|  
|Tailles de champ diffèrent en fonction des relations de clé étrangère/clé primaires.|[!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] ne prend pas en charge la fonctionnalité de Jet de la liaison des colonnes qui ont des types de données différents ou des tailles avec des contraintes de clé étrangère.<br /><br />Lorsque vous convertissez des objets de base de données Access, la fenêtre Sortie affiche des contraintes de clé étrangère/clé primaires qui ne seront pas converties en [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)]. Vous pouvez modifier les tailles sur les colonnes de l’accès et les types de données afin qu’ils correspondent, puis supprimez et rajoutez la base de données Access. Ou bien, vous pouvez migrer les données même si ces contraintes ne seront pas créés dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)].|  
|Les tables référencées dans les relations d’accès ont une clé primaire, ni un index unique.|Access accepte la relation entre les tables où la table référencée n’a pas une clé primaire ou un index unique. Toutefois, cela n’est pas géré par [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)].<br /><br />Lorsque vous convertissez des objets de base de données Access, la fenêtre Sortie répertorie toutes les tables qui n’ont des relations, mais aucune clé primaire ou un index unique. Vous pouvez modifier les tables pour ajouter des clés primaires ou des index uniques, puis supprimez et rajoutez la base de données Access. Ou bien, vous pouvez migrer les données même si la relation entre les tables sera rompue.|  
|Accéder aux tables ont des colonnes de lien hypertexte.|[!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] ne prend pas en charge **lien hypertexte** colonnes. Au lieu de cela, les colonnes sont traités comme des colonnes d’avoir accès. Par défaut, ces colonnes seront convertis en **nvarchar (max)** dans les colonnes [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)]. Vous pouvez personnaliser le mappage. Pour plus d’informations, consultez [Source de mappage et les Types de données cible](http://msdn.microsoft.com/en-us/b362a075-16e7-423f-b63f-e1e9f02844a9).|  
|Expressions de règle de validation ou la valeur par défaut contiennent des fonctions d’accès qui ne peut pas être converties en [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] ou SQL Azure.|Les expressions d’accès par défaut ou les règles de validation peuvent inclure des fonctions d’accès système ou des fonctions définies par l’utilisateur qui ne correspondent pas à [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] ou SQL Azure. À l’aide de fonctions qui ne correspondent pas aux [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] ou SQL Azure ne vous empêche le chargement des règles de validation dans les expressions par défaut [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] ou SQL Azure.|  
  
## <a name="see-also"></a>Voir aussi  
[Préparation pour la Migration des bases de données Access](http://msdn.microsoft.com/en-us/9b80a9e0-08e7-4b4d-b5ec-cc998d3f5114)  
[Migration des bases de données de l’accès à SQL Server](http://msdn.microsoft.com/en-us/76a3abcf-2998-4712-9490-fe8d872c89ca)  
  
