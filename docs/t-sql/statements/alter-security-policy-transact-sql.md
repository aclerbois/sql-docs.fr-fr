---
title: ALTER SECURITY POLICY (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 05/01/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.component: t-sql|statements
ms.reviewer: ''
ms.suite: sql
ms.technology: t-sql
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- ALTER_SECURITY_POLICY_TSQL
- ALTER SECURITY POLICY
- ALTER_SECURITY_TSQL
- ALTER SECURITY
dev_langs:
- TSQL
helpviewer_keywords:
- ALTER SECURITY POLICY statement
ms.assetid: a8efc37e-113d-489c-babc-b914fea2c316
caps.latest.revision: 20
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: 2943e1c1d85e1868ba51433a213e0c6ebfafcdd3
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="alter-security-policy-transact-sql"></a>ALTER SECURITY POLICY (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2016-asdb-xxxx-xxx-md](../../includes/tsql-appliesto-ss2016-asdb-xxxx-xxx-md.md)]

  Modifie une stratégie de sécurité.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```sql  
ALTER SECURITY POLICY schema_name.security_policy_name   
    (  
        { ADD { FILTER | BLOCK } PREDICATE tvf_schema_name.security_predicate_function_name   
           ( { column_name | arguments } [ , …n ] ) ON table_schema_name.table_name   
           [ <block_dml_operation> ]  }   
        | { ALTER { FILTER | BLOCK } PREDICATE tvf_schema_name.new_security_predicate_function_name   
             ( { column_name | arguments } [ , …n ] ) ON table_schema_name.table_name   
           [ <block_dml_operation> ] }  
        | { DROP { FILTER | BLOCK } PREDICATE ON table_schema_name.table_name }   
        | [ <additional_add_alter_drop_predicate_statements> [ , ...n ] ]  
    )    [ WITH ( STATE = { ON | OFF } ]  
    [ NOT FOR REPLICATION ]  
[;]  
  
<block_dml_operation>  
    [ { AFTER { INSERT | UPDATE } }   
    | { BEFORE { UPDATE | DELETE } } ]  
```  
  
## <a name="arguments"></a>Arguments  
 security_policy_name  
 Nom de la stratégie de sécurité. Les noms de stratégie de sécurité doivent respecter les règles applicables aux identificateurs et doivent être uniques dans la base de données et pour son schéma.  
  
 schema_name  
 Nom du schéma auquel appartient la stratégie de sécurité. *schema_name* est obligatoire en raison de la liaison de schéma.  
  
 [ FILTER | BLOCK ]  
 Type de prédicat de sécurité pour la fonction liée à la table cible. Les prédicats FILTER filtrent de manière silencieuse les lignes disponibles pour les opérations de lecture. Les prédicats BLOCK bloquent de façon explicite les opérations d’écriture qui violent la fonction de prédicat.  
  
 tvf_schema_name.security_predicate_function_name  
 Fonction de valeur de table inline à utiliser comme prédicat et à appliquer dans les requêtes portant sur une table cible. Au plus un prédicat de sécurité peut être défini pour une opération DML particulière sur une table donnée. La fonction de valeur de table inline doit avoir été créée à l'aide de l'option SCHEMABINDING.  
  
 { nom_colonne | arguments }  
 Le nom de la colonne ou l'expression utilisé en tant que paramètres de la fonction de prédicat de sécurité. Toutes les colonnes de la table cible peuvent être utilisées comme arguments pour la fonction de prédicat. Les expressions qui incluent des littéraux, les données prédéfinies et les expressions qui utilisent des opérateurs arithmétiques peuvent être utilisées.  
  
 *table_schema_name.table_name*  
 Table cible à laquelle sera appliqué le prédicat de sécurité. Plusieurs stratégies de sécurité désactivées peuvent cibler une même table pour une opération DML particulière, mais une seule peut être activée à un moment donné.  
  
 *\<block_dml_operation>*  
 Opération DML particulière pour laquelle le prédicat BLOCK est appliqué. AFTER spécifie que le prédicat est évalué sur les valeurs des lignes après l’exécution de l’opération DML (INSERT ou UPDATE). AFTER spécifie que le prédicat est évalué sur les valeurs des lignes avant l’exécution de l’opération DML (UPDATE ou DELETE). Si aucune opération n’est spécifiée, le prédicat s’applique à toutes les opérations.  
  
 Vous ne pouvez pas modifier (ALTER) l’opération pour laquelle un prédicat BLOCK est appliqué, car l’opération est utilisée pour identifier le prédicat de façon unique. À la place, vous devez supprimer le prédicat et en ajouter un nouveau pour la nouvelle opération.  
  
 WITH ( STATE = { ON | OFF } )  
 Permet à la stratégie de sécurité d'appliquer ses prédicats de sécurité sur les tables cibles, ou l'empêche d'effectuer cette opération. Sauf indication contraire, la stratégie de sécurité en cours de création est désactivée.  
  
 NOT FOR REPLICATION  
 Indique que la stratégie de sécurité ne doit pas être exécutée quand un agent de réplication modifie l'objet cible. Pour plus d’informations, consultez [Contrôler le comportement de déclencheurs et de contraintes au cours de la synchronisation &#40;programmation Transact-SQL de la réplication&#41;](../../relational-databases/replication/control-behavior-of-triggers-and-constraints-in-synchronization.md).  
  
 table_schema_name.table_name  
 Table cible à laquelle sera appliqué le prédicat de sécurité. Plusieurs stratégies de sécurité désactivées peuvent cibler une même table, mais une seule peut être activée à un moment donné.  
  
## <a name="remarks"></a>Notes   
 L'instruction ALTER SECURITY POLICY fait partie de l'étendue d'une transaction. Si la transaction est restaurée, l'instruction l'est également.  
  
 Quand vous utilisez des fonctions de prédicat avec des tables optimisées en mémoire, les stratégies de sécurité doivent inclure **SCHEMABINDING** et utiliser l’indicateur de compilation **WITH NATIVE_COMPILATION**. L’argument SCHEMABINDING ne peut pas être modifié avec l’instruction ALTER, car elle s’applique à tous les prédicats. Pour changer la liaison de schéma, vous devez supprimer et recréer la stratégie de sécurité.  
  
 Les prédicats BLOCK sont évalués après l’exécution de l’opération DML correspondante. Par conséquent, une requête READ UNCOMMITTED peut voir les valeurs temporaires qui seront restaurées.  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l'autorisation ALTER ANY SECURITY POLICY.  
  
 En outre, les autorisations suivantes sont requises pour chaque prédicat ajouté :  
  
-   Les autorisations SELECT et REFERENCES sur la fonction utilisée en tant que prédicat  
-   L'autorisation REFERENCES sur la table cible liée à la stratégie  
-   L'autorisation REFERENCES sur chaque colonne de la table cible utilisée comme arguments  
  
## <a name="examples"></a>Exemples  
 Les exemples suivants illustrent l’utilisation de la syntaxe **ALTER SECURITY POLICY**. Pour obtenir un exemple de scénario de stratégie de sécurité complet, consultez [Sécurité au niveau des lignes](../../relational-databases/security/row-level-security.md).  
  
### <a name="a-adding-an-additional-predicate-to-a-policy"></a>A. Ajout d'un prédicat à une stratégie  
 La syntaxe suivante modifie une stratégie de sécurité, en ajoutant un prédicat de filtre sur la table `mytable`.  
  
```  
ALTER SECURITY POLICY pol1   
    ADD FILTER PREDICATE schema_preds.SecPredicate(column1)   
    ON myschema.mytable;  
```  
  
### <a name="b-enabling-an-existing-policy"></a>B. Activation d'une stratégie existante  
 L'exemple suivant utilise la syntaxe ALTER pour activer une stratégie de sécurité.  
  
```  
ALTER SECURITY POLICY pol1 WITH ( STATE = ON );  
```  
  
### <a name="c-adding-and-dropping-multiple-predicates"></a>C. Ajout et suppression de plusieurs prédicats  
 La syntaxe suivante modifie une stratégie de sécurité, en ajoutant des prédicats de filtre sur les tables `mytable1` et `mytable3` et en supprimant le prédicat de filtre sur la table `mytable2`.  
  
```  
ALTER SECURITY POLICY pol1  
ADD FILTER PREDICATE schema_preds.SecPredicate1(column1)   
    ON myschema.mytable1,  
DROP FILTER PREDICATE   
    ON myschema.mytable2,  
ADD FILTER PREDICATE schema_preds.SecPredicate2(column2, 1)   
    ON myschema.mytable3;  
```  
  
### <a name="d-changing-the-predicate-on-a-table"></a>D. Modification du prédicat sur une table  
 La syntaxe suivante change le prédicat de filtre existant sur la table mytable en fonction SecPredicate2.  
  
```  
ALTER SECURITY POLICY pol1  
    ALTER FILTER PREDICATE schema_preds.SecPredicate2(column1)  
        ON myschema.mytable;  
```  
  
### <a name="e-changing-a-block-predicate"></a>E. Modification d’un prédicat BLOCK  
 Modification de la fonction de prédicat BLOCK pour une opération sur une table.  
  
```  
ALTER SECURITY POLICY rls.SecPol  
    ALTER BLOCK PREDICATE rls.tenantAccessPredicate_v2(TenantId) 
    ON dbo.Sales AFTER INSERT;  
```  
  
## <a name="see-also"></a> Voir aussi  
 [Sécurité au niveau des lignes](../../relational-databases/security/row-level-security.md)   
 [CREATE SECURITY POLICY &#40;Transact-SQL&#41;](../../t-sql/statements/create-security-policy-transact-sql.md)   
 [DROP SECURITY POLICY &#40;Transact-SQL&#41;](../../t-sql/statements/drop-security-policy-transact-sql.md)   
 [sys.security_policies &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-security-policies-transact-sql.md)   
 [sys.security_predicates &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-security-predicates-transact-sql.md)  
  
  
