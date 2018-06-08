---
title: sp_sequence_get_range (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 08/08/2015
ms.prod: sql
ms.prod_service: database-engine, sql-data-warehouse
ms.component: system-stored-procedures
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- sp_sequence_get_range
- sp_sequence_get_range_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sequence number object, sp_sequence_get_range procedure
- sp_sequence_get_range
ms.assetid: 8ca6b0c6-8d9c-4eee-b02f-51ddffab4492
caps.latest.revision: 19
author: edmacauley
ms.author: edmaca
manager: craigg
monikerRange: = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: 6b34171cf9300454dfe35e6f0961eaacff182b4a
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="spsequencegetrange-transact-sql"></a>sp_sequence_get_range (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2012-asdb-asdw-xxx-md](../../includes/tsql-appliesto-ss2012-asdb-asdw-xxx-md.md)]

  Retourne une plage de valeurs de séquence d'un objet séquence. L'objet séquence génère et émet le nombre de valeurs demandées et fournit l'application avec les métadonnées relatives à la plage.  
  
 Pour plus d’informations sur les numéros de séquence, consultez [numéros de séquence](../../relational-databases/sequence-numbers/sequence-numbers.md).  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_sequence_get_range [ @sequence_name = ] N'<sequence>'   
     , [ @range_size = ] range_size  
     , [ @range_first_value = ] range_first_value OUTPUT   
    [, [ @range_last_value = ] range_last_value OUTPUT ]  
    [, [ @range_cycle_count = ] range_cycle_count OUTPUT ]  
    [, [ @sequence_increment = ] sequence_increment OUTPUT ]  
    [, [ @sequence_min_value = ] sequence_min_value OUTPUT ]  
    [, [ @sequence_max_value = ] sequence_max_value OUTPUT ]  
    [ ; ]  
```  
  
## <a name="arguments"></a>Arguments  
 [ **@sequence_name** =] **N**'*séquence*'  
 Nom de l'objet séquence. Le schéma est facultatif. *sequence_name* est **nvarchar(776)**.  
  
 [ **@range_size** =] *range_size*  
 Nombre de valeurs à extraire de la séquence. **@range_size** est **bigint**.  
  
 [ **@range_first_value** =] *range_first_value*  
 Le paramètre de sortie retourne la première valeur (minimale ou maximale) de l'objet séquence utilisée pour calculer la plage demandée. **@range_first_value** est **sql_variant** avec le même type de base que celui de l’objet séquence utilisé dans la demande.  
  
 [ **@range_last_value** =] *range_last_value*  
 Le paramètre de sortie facultatif retourne la dernière valeur de la plage demandée. **@range_last_value** est **sql_variant** avec le même type de base que celui de l’objet séquence utilisé dans la demande.  
  
 [ **@range_cycle_count** =] range_cycle_count  
 Le paramètre de sortie facultatif retourne le nombre de fois où l'objet séquence a été répété pour retourner la plage demandée. **@range_cycle_count** est **int**.  
  
 [ **@sequence_increment** =] *sequence_increment*  
 Le paramètre de sortie facultatif retourne l'incrément de l'objet séquence utilisé pour calculer la plage demandée. **@sequence_increment** est **sql_variant** avec le même type de base que celui de l’objet séquence utilisé dans la demande.  
  
 [ **@sequence_min_value** =] *sequence_min_value*  
 Le paramètre de sortie facultatif retourne la valeur minimale de l'objet séquence. **@sequence_min_value** est **sql_variant** avec le même type de base que celui de l’objet séquence utilisé dans la demande.  
  
 [ **@sequence_max_value** =] *sequence_max_value*  
 Le paramètre de sortie facultatif retourne la valeur maximale de l'objet séquence. **@sequence_max_value** est **sql_variant** avec le même type de base que celui de l’objet séquence utilisé dans la demande.  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 0 (réussite) ou 1 (échec)  
  
## <a name="remarks"></a>Notes  
 sp_sequence_get_rangeis dans la table système sys. schéma et peut être référencée en tant que sys.sp_sequence_get_range.  
  
### <a name="cycling-sequences"></a>Séquences se répétant  
 Si nécessaire, l'objet séquence se répétera le nombre de fois approprié afin de fournir la plage demandée. Le nombre de répétitions est retourné à l'appelant via le paramètre `@range_cycle_count`.  
  
> [!NOTE]  
>  Lors d'un cycle, un objet séquence redémarre à la valeur minimale dans le cas d'une séquence croissante et à la valeur maximale pour une séquence décroissante, et non pas à la valeur de début de l'objet séquence.  
  
### <a name="non-cycling-sequences"></a>Séquences ne se répétant pas  
 Si le nombre de valeurs dans la plage demandée est supérieur aux valeurs disponibles restantes dans l'objet séquence, la plage demandée n'est pas déduite de l'objet séquence et l'erreur suivante 11732 est retournée :  
  
 `The requested range for sequence object '%.*ls' exceeds the maximum or minimum limit. Retry with a smaller range.`  
  
## <a name="permissions"></a>Autorisations  
 Requiert l'autorisation UPDATE sur l'objet séquence ou le schéma de l'objet séquence.  
  
## <a name="examples"></a>Exemples  
 Les exemples suivants utilisent un objet séquence nommé Test.RangeSeq. Utilisez l’instruction suivante pour créer une séquence Test.RangeSeq.  
  
```  
CREATE SCHEMA Test ;  
GO  
  
CREATE SEQUENCE Test.RangeSeq  
    AS int   
    START WITH 1  
    INCREMENT BY 1  
    MINVALUE 1  
    MAXVALUE 25  
    CYCLE  
    CACHE 10  
;  
```  
  
### <a name="a-retrieving-a-range-of-sequence-values"></a>A. Récupération d'une plage de valeurs de séquence  
 L’instruction suivante obtient quatre numéros de séquence à partir de l’objet de séquence Test.RangeSeq et retourne le premier des nombres à l’utilisateur.  
  
```  
DECLARE @range_first_value sql_variant ,   
        @range_first_value_output sql_variant ;  
  
EXEC sp_sequence_get_range  
@sequence_name = N'Test.RangeSeq'  
, @range_size = 4  
, @range_first_value = @range_first_value_output OUTPUT ;  
  
SELECT @range_first_value_output AS FirstNumber ;  
  
```  
  
### <a name="b-returning-all-output-parameters"></a>B. Retour de tous les paramètres de sortie  
 L’exemple suivant retourne toutes les valeurs de sortie de la procédure sp_sequence_get_range.  
  
```  
DECLARE    
  @FirstSeqNum sql_variant  
, @LastSeqNum sql_variant  
, @CycleCount int  
, @SeqIncr sql_variant  
, @SeqMinVal sql_variant  
, @SeqMaxVal sql_variant ;  
  
EXEC sys.sp_sequence_get_range  
@sequence_name = N'Test.RangeSeq'  
, @range_size = 5  
, @range_first_value = @FirstSeqNum OUTPUT   
, @range_last_value = @LastSeqNum OUTPUT   
, @range_cycle_count = @CycleCount OUTPUT  
, @sequence_increment = @SeqIncr OUTPUT  
, @sequence_min_value = @SeqMinVal OUTPUT  
, @sequence_max_value = @SeqMaxVal OUTPUT ;  
  
-- The following statement returns the output values  
SELECT  
  @FirstSeqNum AS FirstVal  
, @LastSeqNum AS LastVal  
, @CycleCount AS CycleCount  
, @SeqIncr AS SeqIncrement  
, @SeqMinVal AS MinSeq  
, @SeqMaxVal AS MaxSeq ;  
  
```  
  
 La modification de l'argument `@range_size` en nombre plus grand, comme 75, provoque l'exécution répétée de l'objet séquence. Vérifiez l'argument `@range_cycle_count` pour déterminer si et combien de fois l'objet séquence a été répété.  
  
### <a name="c-example-using-adonet"></a>C. Exemple utilisant ADO.NET  
 L’exemple suivant obtient une plage à partir de la Test.RangeSeq à l’aide d’ADO.NET.  
  
```  
SqlCommand cmd = new SqlCommand();  
cmd.Connection = conn;  
cmd.CommandType = CommandType.StoredProcedure;  
cmd.CommandText = "sys.sp_sequence_get_range";  
cmd.Parameters.AddWithValue("@sequence_name", "Test.RangeSeq");  
cmd.Parameters.AddWithValue("@range_size", 10);  
  
// Specify an output parameter to retreive the first value of the generated range.  
SqlParameter firstValueInRange = new SqlParameter("@range_first_value", SqlDbType.Variant);  
firstValueInRange.Direction = ParameterDirection.Output;  
cmd.Parameters.Add(firstValueInRange);  
  
conn.Open();  
cmd.ExecuteNonQuery();  
  
// Output the first value of the generated range.  
Console.WriteLine(firstValueInRange.Value);  
  
```  
  
## <a name="see-also"></a>Voir aussi  
 [CREATE SEQUENCE &#40;Transact-SQL&#41;](../../t-sql/statements/create-sequence-transact-sql.md)   
 [ALTER SEQUENCE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-sequence-transact-sql.md)   
 [DROP SEQUENCE &#40;Transact-SQL&#41;](../../t-sql/statements/drop-sequence-transact-sql.md)   
 [NEXT VALUE FOR &#40;Transact-SQL&#41;](../../t-sql/functions/next-value-for-transact-sql.md)   
 [Numéros de séquence](../../relational-databases/sequence-numbers/sequence-numbers.md)  
  
  
