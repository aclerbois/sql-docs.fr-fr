---
title: Déclencheurs DML | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.component: triggers
ms.reviewer: ''
ms.suite: sql
ms.technology:
- dbe-dml
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- triggers [SQL Server], about triggers
- DML triggers, about DML triggers
- triggers [SQL Server]
ms.assetid: 298eafca-e01f-4707-8c29-c75546fcd6b0
caps.latest.revision: 27
author: rothja
ms.author: jroth
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: b102242b35b67ec8126b6286ded3ec1d7e0351c5
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="dml-triggers"></a>Déclencheurs DML
[!INCLUDE[tsql-appliesto-ss2008-asdb-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-asdb-xxxx-xxx-md.md)]
  Un déclencheur DML est un type spécial de procédure stockée qui entre automatiquement en vigueur lorsqu'un événement de langage de manipulation de données (DML, ou Data Manipulation Language) qui affecte la table ou la vue définie dans le déclencheur se produit. Les événements DML incluent les instructions INSERT, UPDATE ou DELETE. Les déclencheurs DML peuvent être utilisés pour appliquer des règles d'entreprise et l'intégrité des données, interroger d'autres tables et inclure des instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] complexes. Le déclencheur et l'instruction qui le déclenche sont traités comme une unique transaction qui peut être annulée (par une opération de restauration) à partir du déclencheur. Si une erreur grave est détectée (par exemple un espace disque insuffisant), toute la transaction est automatiquement annulée.  
  
## <a name="dml-trigger-benefits"></a>Avantages des déclencheurs DML  
 Les déclencheurs DML sont semblables aux contraintes en ce sens qu'ils peuvent appliquer l'intégrité d'entité ou l'intégrité de domaine. En général, l'intégrité d'entité doit toujours être appliquée au niveau le plus bas par des index qui font partie des contraintes PRIMARY KEY et UNIQUE, ou qui sont créés indépendamment des contraintes. L'intégrité de domaine doit être appliquée au moyen de contraintes CHECK, et l'intégrité référentielle au moyen de contraintes FOREIGN KEY. Les déclencheurs DML sont particulièrement utiles lorsque les fonctionnalités prises en charge par les contraintes ne peuvent pas répondre aux besoins fonctionnels de l'application.  
  
 La liste suivante compare les déclencheurs DML aux contraintes et identifie les situations dans lesquelles les déclencheurs DML présentent plus d’avantages.  
  
-   Les déclencheurs DML peuvent effectuer des modifications en cascade dans des tables associées dans la base de données, mais ces modifications peuvent être exécutées plus efficacement par le biais de contraintes d'intégrité référentielle en cascade. Les contraintes FOREIGN KEY peuvent valider une valeur de colonne uniquement si celle-ci correspond exactement à une valeur d'une autre colonne, à moins que la clause REFERENCES définisse une action d'intégrité référentielle en cascade.  
  
-   Ils peuvent empêcher les opérations INSERT, UPDATE et DELETE incorrectes et assurer l'application d'autres restrictions plus complexes que celles définies à l'aide de contraintes CHECK.  
  
     Contrairement aux contraintes CHECK, les déclencheurs DML peuvent faire référence à des colonnes d'autres tables. Par exemple, un déclencheur peut utiliser une instruction SELECT à partir d'une autre table afin de comparer avec les données insérées ou mises à jour et d'effectuer des actions supplémentaires, comme modifier les données ou afficher un message d'erreur défini par l'utilisateur.  
  
-   Ils peuvent déterminer l'état d'une table avant et après une modification de données, et entreprendre des actions en fonction de cette différence d'état.  
  
-   Plusieurs déclencheurs DML du même type (INSERT, UPDATE ou DELETE) sur une table permettent que plusieurs actions différentes soient réalisées en réponse à la même instruction de modification.  
  
-   Les contraintes ne peuvent donner d'informations sur les erreurs que par l'intermédiaire des messages d'erreur système standard. Si votre application a besoin (ou peut tirer parti) de messages personnalisés et d'une gestion des erreurs plus complexe, vous devez utiliser un déclencheur.  
  
-   Les déclencheurs DML peuvent servir à interdire ou à restaurer des modifications qui enfreignent l'intégrité référentielle, annulant ainsi la tentative de modification des données. Un déclencheur de ce type peut entrer en action si vous modifiez une clé étrangère et si la nouvelle valeur ne correspond pas à sa clé primaire. Toutefois, les contraintes FOREIGN KEY sont généralement utilisées à cette fin.  
  
-   Si des contraintes existent sur la table du déclencheur, elles sont vérifiées après l'exécution du déclencheur INSTEAD OF mais avant celle du déclencheur AFTER. Si les contraintes sont violées, les actions du déclencheur INSTEAD OF sont restaurées et le déclencheur AFTER n'est pas exécuté.  
  
## <a name="types-of-dml-triggers"></a>Types de déclencheurs DML  
 Déclencheur AFTER  
 Les déclencheurs AFTER sont exécutés après l'action associée à une instruction INSERT, UPDATE, MERGE ou DELETE. Les déclencheurs AFTER ne sont jamais exécutés en cas de violation de contrainte et ne peuvent donc pas être utilisés pour tout traitement susceptible d'éviter les violations de contrainte. Pour chaque instruction INSERT, UPDATE ou DELETE spécifiée dans une instruction MERGE, le déclencheur correspondant est exécuté pour chaque opération DML.  
  
 Déclencheur INSTEAD OF  
 Les déclencheurs INSTEAD OF remplacent les actions standard de l'instruction de déclenchement. Par conséquent, ils peuvent être utilisés pour effectuer une vérification d'erreur ou de valeur sur une ou plusieurs colonnes, puis effectuer des actions supplémentaires avant l'insertion, la mise à jour ou la suppression des lignes. Par exemple, lorsque la valeur mise à jour dans une colonne de salaire horaire dans une table de registre du personnel dépasse une valeur spécifiée, il est possible de définir un déclencheur qui soit produit un message d'erreur et annule la transaction, soit insère un nouvel enregistrement dans un journal d'audit avant d'insérer l'enregistrement dans la table de registre du personnel. Le principal avantage des déclencheurs INSTEAD OF est qu'ils autorisent des vues qui autrement ne pourraient pas être mises à jour en vue de la prise en charge des mises à jour. Par exemple, une vue basée sur plusieurs tables de base doit posséder un déclencheur INSTEAD OF pour prendre en charge les opérations d'insertion, de mise à jour et de suppression référençant des données dans plusieurs tables. Un autre avantage des déclencheurs INSTEAD OF est qu'ils permettent de programmer une logique capable de rejeter certaines parties d'un lot et d'en mener d'autres à terme.  
  
 Le tableau suivant compare les fonctionnalités des déclencheurs AFTER et INSTEAD OF.  
  
|Fonction|Déclencheur AFTER|Déclencheur INSTEAD OF|  
|--------------|-------------------|------------------------|  
|Applicabilité|Tables|Tables et vues|  
|Quantité par table ou vue|Plusieurs par action de déclenchement (UPDATE, DELETE et INSERT)|Un par action de déclenchement (UPDATE, DELETE et INSERT)|  
|Références en cascade|Aucune restriction|Les déclencheurs INSTEAD OF UPDATE et DELETE ne sont pas autorisés sur des tables qui sont des cibles de contraintes d'intégrité référentielle en cascade.|  
|Exécution|Après :<br /><br /> Traitement des contraintes<br /><br /> Actions référentielles déclaratives<br /><br /> Création de tables**inserted** et **deleted** <br /><br /> L'action de déclenchement|Avant : Traitement des contraintes<br /><br /> Au lieu de : L’action de déclenchement<br /><br /> Après : Création de tables  **inserted** et **deleted**|  
|Ordre d'exécution|La première et la dernière exécution peuvent être spécifiées|Non applicable|  
|Références de colonnes**varchar(max)**, **nvarchar(max)** et **varbinary(max)** dans des  **inserted** et **deleted** |Autorisé|Autorisé|  
|Références de colonnes**text**, **ntext**et **image** dans des  **inserted** et **deleted** |Non autorisées|Autorisé|  
  
 Déclencheurs CLR  
 Un déclencheur CLR peut être un déclencheur AFTER ou INSTEAD OF. Il peut également s'agir d'un déclencheur DDL. Au lieu d'exécuter une procédure stockée [!INCLUDE[tsql](../../includes/tsql-md.md)] , un déclencheur CLR exécute une ou plusieurs méthodes écrites en code managé que les membres d'un assembly ont créées dans .NET Framework et téléchargées dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
## <a name="related-tasks"></a>Related Tasks  
  
|Tâche|Rubrique|  
|----------|-----------|  
|Décrit comment créer un déclencheur DML.|[Créer des déclencheurs DML](../../relational-databases/triggers/create-dml-triggers.md)|  
|Décrit comment créer un déclencheur CLR.|[Créer des déclencheurs CLR](../../relational-databases/triggers/create-clr-triggers.md)|  
|Décrit comment créer un déclencheur DML pour gérer à la fois les modifications de données portant sur une seule ligne et plusieurs lignes.|[Créer de déclencheurs DML pour gérer plusieurs lignes de données](../../relational-databases/triggers/create-dml-triggers-to-handle-multiple-rows-of-data.md)|  
|Explique comment imbriquer des déclencheurs.|[Créer des déclencheurs imbriqués](../../relational-databases/triggers/create-nested-triggers.md)|  
|Décrit comment spécifier l'ordre dans lequel les déclencheurs AFTER sont activés.|[Spécifier les premier et dernier déclencheurs](../../relational-databases/triggers/specify-first-and-last-triggers.md)|  
|Décrit comment utiliser les tables inserted et deleted spéciales dans le code de déclencheur.|[Utiliser les tables inserted et deleted](../../relational-databases/triggers/use-the-inserted-and-deleted-tables.md)|  
|Explique comment modifier ou renommer un déclencheur DML.|[Modifier ou renommer les déclencheurs DML](../../relational-databases/triggers/modify-or-rename-dml-triggers.md)|  
|Décrit comment afficher des informations sur les déclencheurs DML.|[Obtenir des informations sur les déclencheurs DML](../../relational-databases/triggers/get-information-about-dml-triggers.md)|  
|Décrit comment supprimer ou désactiver les déclencheurs DML.|[Supprimer ou désactiver les déclencheurs DML](../../relational-databases/triggers/delete-or-disable-dml-triggers.md)|  
|Décrit comment gérer la sécurité du déclencheur.|[Gérer la sécurité des déclencheurs](../../relational-databases/triggers/manage-trigger-security.md)|  
  
## <a name="see-also"></a> Voir aussi  
 [CREATE TRIGGER &#40;Transact-SQL&#41;](../../t-sql/statements/create-trigger-transact-sql.md)   
 [ALTER TRIGGER &#40;Transact-SQL&#41;](../../t-sql/statements/alter-trigger-transact-sql.md)   
 [DROP TRIGGER &#40;Transact-SQL&#41;](../../t-sql/statements/drop-trigger-transact-sql.md)   
 [DISABLE TRIGGER &#40;Transact-SQL&#41;](../../t-sql/statements/disable-trigger-transact-sql.md)   
 [Fonctions de déclencheur &#40;Transact-SQL&#41;](../../t-sql/functions/trigger-functions-transact-sql.md)  
  
  
