---
title: capture de données modifiées. &lt;capture_instance&gt;_CT (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 05/01/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: system-tables
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- cdc
- cdc_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- cdc.<capture_instance>_CT
ms.assetid: 979c8110-3c54-4e76-953c-777194bc9751
caps.latest.revision: 27
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: 7e2ef0fa035acb3bc87e5bcebb7fb5dc129d5388
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="cdcltcaptureinstancegtct-transact-sql"></a>capture de données modifiées. &lt;capture_instance&gt;_CT (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Table de modifications créée lorsque la capture de données modifiées est activée sur une table source. La table retourne une ligne pour chaque opération d'insertion et de suppression effectuée sur la table source, et deux lignes pour chaque opération de mise à jour effectuée sur la table source. Lorsque le nom de la table de modifications n'est pas spécifié au moment de l'activation de la table source, le nom est dérivé. Le format du nom est cdc. *capture_instance*_CT où *capture_instance* est le nom de schéma de la table source et le nom de la table source au format *schema_table*. Par exemple, si la table **Person.Address** dans les **AdventureWorks** base de données exemple est activée pour la capture de données modifiées, le nom de la table de modifications dérivé serait **capture de données modifiées. Person_Address_CT**.  
  
 Il est recommandé que vous **ne pas interroger directement les tables système**. À la place, exécutez le [cdc.fn_cdc_get_all_changes_ < instance_capture >](../../relational-databases/system-functions/cdc-fn-cdc-get-all-changes-capture-instance-transact-sql.md) et [cdc.fn_cdc_get_net_changes_ < instance_capture >](../../relational-databases/system-functions/cdc-fn-cdc-get-net-changes-capture-instance-transact-sql.md) fonctions.  
  

  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|**__$start_lsn**|**binary(10)**|Numéro séquentiel dans le journal associé à la transaction de validation pour la modification.<br /><br /> Toutes les modifications validées dans la même transaction partagent le même numéro séquentiel dans le journal de validation. Par exemple, si une opération de suppression sur la table source supprime deux lignes, la table de modifications contiendra deux lignes, chacune avec la même **__ $start_lsn** valeur.|  
|**__ $end_lsn**|**binary(10)**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]<br /><br /> Dans [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)], cette colonne a toujours pour valeur NULL.|  
|**__$seqval**|**binary(10)**|Valeur de séquence utilisée pour classer les modifications de ligne dans une transaction.|  
|**__$operation**|**int**|Identifie l'opération de langage de manipulation de données associée à la modification. Les valeurs possibles sont les suivantes :<br /><br /> 1 = suppression<br /><br /> 2 = insertion<br /><br /> 3 = mise à jour (anciennes valeurs)<br /><br /> Les données de colonne ont des valeurs de ligne avant d'exécuter l'instruction UPDATE.<br /><br /> 4 = mise à jour (nouvelles valeurs)<br /><br /> Les données de colonne ont des valeurs de ligne après l'exécution de l'instruction UPDATE.|  
|**__$update_mask**|**varbinary(128)**|Un masque de bits basé sur les ordinaux de colonne de la table de modifications identifiant les colonnes modifiées.|  
|*\<<colonnes_de_table_source_capturées>*|variable|Les colonnes restantes de la table de modifications sont les colonnes de la table source qui ont été identifiées comme colonnes capturées lorsque l'instance de capture a été créée. Si aucune colonne n'a été spécifiée dans la liste des colonnes capturées, toutes les colonnes de la table source sont incluses dans cette table.|  
|**__ $id_de_commande** |**int** |Effectue le suivi de l’ordre des opérations dans une transaction. |  
  
## <a name="remarks"></a>Notes  

Le `__$command_id` une colonne a une colonne a été introduite dans une mise à jour cumulative dans les versions 2012 2016. Pour plus d’informations de version et de téléchargement, consultez l’article 3030352 à [FIX : la table de modifications est ordonnée incorrecte pour la mise à jour de lignes après avoir activé la modification de données de capture pour une base de données Microsoft SQL Server](https://support.microsoft.com/help/3030352/fix-the-change-table-is-ordered-incorrectly-for-updated-rows-after-you).  Pour plus d’informations, consultez [les fonctionnalités de capture de données modifiées peuvent interrompre après la mise à niveau vers la dernière CU pour SQL Server 2012, 2014 et 2016](https://blogs.msdn.microsoft.com/sql_server_team/cdc-functionality-may-break-after-upgrading-to-the-latest-cu-for-sql-server-2012-2014-and-2016/).

## <a name="captured-column-data-types"></a>Types de données de la colonne capturée.  
 Les colonnes capturées incluses dans cette table ont les mêmes type de données et valeur que leurs colonnes sources correspondantes avec les exceptions suivantes :  
  
-   **Horodatage** les colonnes sont définies en tant que **Binary (8)**.  
  
-   **Identité** les colonnes sont définies en tant que **int** ou **bigint**.  
  
 Toutefois, les valeurs dans ces colonnes sont les mêmes que celles des colonnes sources.  
  
### <a name="large-object-data-types"></a>Types de données des objets importants  
 Colonnes de type de données **image**, **texte**, et **ntext** sont toujours attribués une **NULL** valeur lorsque __ $operation = 1 ou \_ \_$operation = 3. Colonnes de type de données **varbinary (max)**, **varchar (max)**, ou **nvarchar (max)** assignés un **NULL** valeur lorsque \_ \_$operation = 3, sauf si la colonne a changé pendant la mise à jour. Lorsque \_ \_$operation = 1, ces colonnes sont affectées de leur valeur au moment de la suppression. Les colonnes calculées qui sont inclus dans une instance de capture toujours ont la valeur **NULL**.  
  
 Par défaut, la taille maximale qui peut être ajoutée à une colonne capturée dans une seule instruction INSERT, UPDATE, WRITETEXT ou UPDATETEXT est de 65 536 octets ou 64 Ko. Pour augmenter cette taille afin de prendre en charge davantage de données LOB, utilisez le [configurer l’Option de Configuration de serveur de max text repl size](../../database-engine/configure-windows/configure-the-max-text-repl-size-server-configuration-option.md) pour spécifier une plus grande taille maximale. Pour plus d’informations, consultez [Configurer l’option de configuration du serveur max text repl size](../../database-engine/configure-windows/configure-the-max-text-repl-size-server-configuration-option.md).  
  
## <a name="data-definition-language-modifications"></a>Modifications du langage de définition de données (DDL)  
 Modifications DDL apportées à la table source, telles que l’ajout ou la suppression de colonnes, sont enregistrées dans le [cdc.ddl_history](../../relational-databases/system-tables/cdc-ddl-history-transact-sql.md) table. Ces modifications ne sont pas appliquées à la table de modifications. Autrement dit, la définition de la table de modifications reste constante. Lors de l'insertion de lignes dans la table de modifications, le processus de capture ignore les colonnes qui n'apparaissent pas dans la liste des colonnes capturées associées à la table source. Si une colonne apparaît dans la liste des colonnes capturées qui n'est plus dans la table source, une valeur Null est assignée à la colonne.  
  
 Modification du type de données d’une colonne dans la table source est également enregistrée dans le [cdc.ddl_history](../../relational-databases/system-tables/cdc-ddl-history-transact-sql.md) table. Toutefois, cette modification altère la définition de la table de modifications. Le type de données de la colonne capturée dans la table de modifications est modifié lorsque le processus de capture rencontre l'enregistrement du journal pour la modification DDL apportée à la table source.  
  
 Si vous devez modifier le type de données d'une colonne capturée dans la table source d'une façon qui réduit la taille du type de données, utilisez la procédure suivante pour vous assurer que la colonne équivalente dans la table de modifications peut être correctement modifiée.  
  
1.  Dans la table source, mettez à jour les valeurs dans la colonne à modifier pour qu'elles s'ajustent à la taille de type de données planifiée. Par exemple, si vous modifiez le type de données à partir de **int** à **smallint**, mettre à jour les valeurs à une taille qui correspond à la **smallint** plage de-32 768 à 32 767.  
  
2.  Dans la table de modifications, effectuez la même opération de mise à jour sur la colonne équivalente.  
  
3.  Modifiez la table source en spécifiant le nouveau type de données. La modification du type de données est propagée avec succès à la table de modifications.  
  
## <a name="data-manipulation-language-modifications"></a>Modifications du langage de manipulation de données  
 Lorsque des opérations d'insertion, de mise à jour et de suppression sont effectuées sur une table source où la capture de données modifiées est activée, un enregistrement de ces opérations DML apparaît dans le journal des transactions de la base de données. Le processus de capture de la capture de données modifiées récupère les informations sur ces modifications dans le journal des transactions et ajoute une ou deux lignes à la table de modifications pour enregistrer la modification. Les entrées sont ajoutées à la table de modifications selon l'ordre dans lequel elles ont été validées dans la table source, même si la validation d'entrées de table de modifications doit en général être effectuée sur un groupe de modifications plutôt que sur une entrée unique.  
  
 Dans l’entrée de table de modifications, la **__ $start_lsn** colonne est utilisée pour enregistrer la numéro LSN qui est associé à la modification apportée à la table source, de validation et la **colonne de __ $seqval** est utilisée pour classer la modification dans sa transaction. Ensemble, ces colonnes de métadonnées peuvent être utilisées pour faire en sorte que l'ordre de validation des modifications de source soit conservé. Dans la mesure où le processus de capture obtient ses informations de modification du journal des transactions, il est important de noter que les entrées de table de modifications n'apparaissent pas de façon synchrone avec leurs modifications correspondantes de la table source. Les modifications apparaissent en effet de façon asynchrone, une fois que le processus de capture a traité les entrées de modification pertinentes du journal des transactions.  
  
 Pour les opérations d'insertion et de suppression, tous les bits du masque de mise à jour sont définis. Pour les opérations de mise à jour, le masque de mise à jour sera modifié dans les lignes de mise à jour nouvelles et anciennes pour refléter les colonnes qui ont changé pendant la mise à jour.  
  
## <a name="see-also"></a>Voir aussi  
 [Sys.sp_cdc_enable_table &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sys-sp-cdc-enable-table-transact-sql.md)   
 [Sys.sp_cdc_get_ddl_history &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sys-sp-cdc-get-ddl-history-transact-sql.md)  
  
  
