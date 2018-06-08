---
title: Sys.fn_net_changes_&lt;capture_instance&gt; (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: system-functions
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
applies_to:
- SQL Server (starting with 2008)
f1_keywords:
- sys.fn_net_changes_TSQL
- fn_net_changes_TSQL
- fn_net_changes
- sys.fn_net_changes
dev_langs:
- TSQL
helpviewer_keywords:
- fn_net_changes_<capture_instance>
- sys.fn_net_changes_<capture_instance>
ms.assetid: 342fa030-9fd9-4b74-ae4d-49f6038a5073
caps.latest.revision: 16
author: rothja
ms.author: jroth
manager: craigg
ms.openlocfilehash: 36430bb3303088677debcd65b2e1f10f170ed26b
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="sysfnnetchangesltcaptureinstancegt-transact-sql"></a>sys.fn_net_changes_&lt;capture_instance&gt; (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Wrappers pour le **net modifications** fonctions de requête. Les scripts requis pour créer ces fonctions sont générés par la procédure stockée sys.sp_cdc_generate_wrapper_function.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
fn_net_changes_<capture_instance> ('start_time', 'end_time', '<row_filter_option>' )  
  
<capture_instance> ::= The name of the capture instance.  
<row_filter_option> ::=  
{ all  
  | all with mask  
  | all with merge  
}  
```  
  
## <a name="arguments"></a>Arguments  
 *start_time*  
 Le **datetime** valeur qui représente le point de terminaison inférieur de la plage d’entrées de table de modifications à inclure dans le jeu de résultats.  
  
 Seules les lignes dans la capture de données modifiées. < instance_capture > modifications de table qui ont une heure de validation associée strictement supérieure à *heure_début* sont inclus dans le jeu de résultats.  
  
 Si une valeur NULL est fournie pour cet argument, le point de terminaison inférieur de la plage de requêtes correspond au point de terminaison inférieur de la plage valide pour l'instance de capture.  
  
 *end_time*  
 Le **datetime** valeur qui représente le point de terminaison supérieur de la plage d’entrées de table de modifications à inclure dans le jeu de résultats.  
  
 Ce paramètre peut prendre l’une des deux significations, selon la valeur choisie pour @closed_high_end_point lorsque sys.sp_cdc_generate_wrapper_function est appelé pour générer le script pour créer la fonction wrapper :  
  
-   @closed_high_end_point = 1  
  
     Uniquement les lignes dans la capture de données modifiées. < instance_capture > table de modifications qui ont une valeur dans \_ \_$start_lsn et une validation correspondante de temps inférieur ou égal à **heure_début** sont inclus dans le jeu de résultats.  
  
-   @closed_high_end_point = 0  
  
     Uniquement les lignes dans la capture de données modifiées. < instance_capture > table de modifications qui ont une valeur dans \_ \_$start_lsn et une heure de validation strictement inférieure à **heure_début** sont inclus dans le jeu de résultats.  
  
 Si une valeur NULL est fournie pour cet argument, le point de terminaison supérieur de la plage de requêtes correspond au point de terminaison supérieur de la plage valide pour l'instance de capture.  
  
 *< row_filter_option >* :: = {toutes les | tous dotés du masque | all avec fusion}  
 Option qui régit le contenu des colonnes de métadonnées aussi bien que les lignes retournées dans le jeu de résultats. Il peut s'agir de l'une des options suivantes :  
  
 all  
 Retourne le contenu final d'une ligne modifiée dans les colonnes de contenu ainsi que l'opération requise pour appliquer la ligne dans la colonne de métadonnées __CDC_OPERATION.  
  
 all with mask  
 Retourne le contenu final de toutes les lignes modifiées dans les colonnes de contenu ainsi que l'opération nécessaire pour appliquer chaque ligne dans la colonne de métadonnées __CDC_OPERATION. Si une liste d'indicateurs de mise à jour a été spécifiée quand vous avez généré le script pour créer la fonction wrapper, cette option est requise pour remplir le masque de mise à jour.  
  
 all with merge  
 Retourne le contenu final de toutes les lignes modifiées dans les colonnes de contenu.  
  
 La colonne __CDC_OPERATION aura l'une des deux valeurs suivantes :  
  
-   D si la ligne doit être supprimée.  
  
-   M si la ligne doit être insérée ou mise à jour.  
  
 La logique permettant de déterminer si une insertion ou une mise à jour est nécessaire pour appliquer une modification à la cible ajoute une dose de complexité à la requête. Utilisez cette option pour améliorer les performances lorsqu'il n'est pas nécessaire d'effectuer la distinction entre les opérations d'insertion et de mise à jour. Cette approche est la mieux adaptée dans les environnements cibles où une opération de fusion est disponible directement, tel qu'un environnement [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].  
  
## <a name="table-returned"></a>Table retournée  
  
|Nom de colonne|Type de colonne| Description|  
|-----------------|-----------------|-----------------|  
|\<colonnes de @column_list>|**Varie**|Les colonnes qui sont identifiées dans le **column_list** argument de sp_cdc_generate_wrapper_function lorsqu’elle est appelée pour générer le script pour créer le wrapper. Si *column_list* est NULL, toutes les colonnes sources suivies apparaîtront dans le jeu de résultats.|  
|__CDC_OPERATION|**nvarchar(2)**|Code d'opération qui indique quelle opération est requise pour appliquer la ligne à l'environnement cible. L’opération varie selon la valeur de l’argument *row_filter_option* qui est fourni dans l’appel suivant :<br /><br /> *row_filter_option* = « all », « all with mask'<br /><br /> 'D' - opération de suppression<br /><br /> 'I' - opération d'insertion<br /><br /> 'UN' - opération de mise à jour<br /><br /> *row_filter_option* = « all with merge'<br /><br /> 'D' - opération de suppression<br /><br /> 'M' – opération d'insertion ou de mise à jour|  
|\<colonnes de @update_flag_list>|**bit**|Indicateur de bit nommé en ajoutant _uflag au nom de colonne. L’indicateur prend une valeur non null uniquement lorsque *row_filter_option* **= « all with mask'** et \__CDC_OPERATION **bien ='**. Il est défini à 1 si la colonne correspondante a été modifiée dans la fenêtre de requête. Sinon, il prend la valeur 0.|  
  
## <a name="remarks"></a>Notes  
 La fonction fn_net_changes_<instance_capture> sert de wrapper pour la fonction de requête cdc.fn_cdc_get_net_changes_<instance_capture>. La procédure stockée sys.sp_cdc_generate_wrapper est utilisée pour créer le script du wrapper.  
  
 Les fonctions wrapper ne sont pas créées automatiquement. Vous devez effectuer deux opérations pour créer des fonctions wrapper :  
  
1.  Exécuter la procédure stockée pour générer le script pour créer le wrapper.  
  
2.  Exécuter le script pour créer la fonction wrapper.  
  
 Fonctions wrapper permettent aux utilisateurs d’interroger systématiquement les modifications qui se sont produites dans un intervalle lié par **datetime** valeurs au lieu de valeurs LSN. Les fonctions wrapper effectuent toutes les conversions requises entre les **datetime** valeurs et les valeurs LSN requises en interne en tant qu’arguments pour les fonctions de requête. Lorsque les fonctions wrappers sont utilisées en série pour traiter un flux de données modifiées, elles garantissent qu’aucune perte ou de données répétées autant que la convention suivante soit respectée : la @end_time valeur de l’intervalle associé à un appel est fournie en tant que le @start_time valeur pour l’intervalle associé à l’appel suivant.  
  
 L'utilisation du paramètre @closed_high_end_point lors de la création du script vous permet de générer des wrappers destinés à prendre en charge une limite supérieure fermée ou une limite supérieure ouverte sur la fenêtre de requête spécifiée. Autrement dit, vous pouvez décider si les entrées qui ont une heure de validation égale à la limite supérieure de l'intervalle d'extraction doivent être incluses dans l'intervalle. Par défaut, la limite supérieure est incluse.  
  
 Le jeu de résultats retourné par le **net modifications** wrapper fonction retourne uniquement les colonnes suivies présentes dans le @column_list lorsque le wrapper a été généré. Si @column_list a la valeur NULL, toutes les colonnes sources suivies sont retournées. Les colonnes sources sont suivies d'une colonne d'opération, __CDC_OPERATION, qui est une colonne à un ou deux caractères qui identifie l'opération.  
  
 Indicateurs de bits sont ensuite rajoutés au jeu de résultats pour chaque colonne identifiée dans le paramètre @update_flag_list. Pour le **net modifications** wrapper, les bits indicateurs sont toujours NULL si le @row_filter_option qui est utilisé dans l’appel à la fonction wrapper est 'all' ou « all with merge ». Si le @row_filter_option a la valeur 'all with mask' et __CDC_OPERATION est ' ou 'I', la valeur de l’indicateur est également NULL. Si \__CDC_OPERATION est bien », l’indicateur a la valeur 1 ou 0, selon que le **net** opération a provoqué une modification de la colonne mise à jour.  
  
 Le modèle de configuration de capture de données modifiées 'Instantiate CDC Wrapper TVFs for Schema' illustre comment utiliser la procédure stockée sp_cdc_generate_wrapper_function pour obtenir des scripts CREATE pour toutes les fonctions wrapper pour les fonctions de requêtes définies d'un schéma. Le modèle crée ensuite ces scripts. Pour plus d’informations sur les modèles, consultez [l’Explorateur de modèles](http://msdn.microsoft.com/library/b9ee55c5-bb44-4f76-90ac-792d8d83b4c8).  
  
## <a name="see-also"></a>Voir aussi  
 [Sys.sp_cdc_generate_wrapper_function &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sys-sp-cdc-generate-wrapper-function-transact-sql.md)   
 [cdc.fn_cdc_get_net_changes_&#60;capture_instance&#62; &#40;Transact-SQL&#41;](../../relational-databases/system-functions/cdc-fn-cdc-get-net-changes-capture-instance-transact-sql.md)  
  
  
