---
title: sp_fulltext_service (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: system-stored-procedures
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- sp_fulltext_service
- sp_fulltext_service_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- full-text search [SQL Server], properties
- sp_fulltext_service
- Full-Text Search Upgrade Option
ms.assetid: 17a91433-f9b6-4a40-88c4-8c704ec2de9f
caps.latest.revision: 79
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 5c0000f2340f83e943b329a2bf27fc725938c7f2
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="spfulltextservice-transact-sql"></a>sp_fulltext_service (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Modifie les propriétés serveur de la recherche en texte intégral pour [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_fulltext_service [ [@action=] 'action'   
     [ , [ @value= ] value ] ]  
```  
  
## <a name="arguments"></a>Arguments  
 [  **@action=**] **'***action***'**  
 Propriété à modifier ou à réinitialiser. *action* est **nvarchar(100),** sans valeur par défaut. Pour obtenir la liste d’un*c*tion propriétés, leurs descriptions et les valeurs qui peuvent être définies, consultez le tableau sous la *valeur* argument. Cet argument retourne les propriétés suivantes : type de données, valeur d'exécution actuelle, valeur minimum ou maximum et état de désapprobation, le cas échéant.  
  
 [  **@value=**] *valeur*  
 Valeur de la propriété spécifiée. *valeur* est **sql_variant**, avec NULL comme valeur par défaut. Si @value a la valeur null, **sp_fulltext_service** retourne le paramètre actuel. Ce tableau répertorie les propriétés relatives aux actions, leurs descriptions et les valeurs qui peuvent être définies.  
  
> [!NOTE]  
>  Les actions suivantes seront supprimées dans une future version de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]: **clean_up**, **connect_timeout**, **data_timeout**, et **resource_usage**. Évitez par conséquent d'utiliser ces actions dans un nouveau travail de développement et prévoyez la modification des applications qui les utilisent actuellement.  
  
|Action|Type de données| Description|  
|------------|---------------|-----------------|  
|**clean_up**|**int**|Pris en charge pour la compatibilité descendante uniquement. La valeur est toujours 0.|  
|**connect_timeout**|**int**|Pris en charge pour la compatibilité descendante uniquement. La valeur est toujours 0.|  
|**data_timeout**|**int**|Pris en charge pour la compatibilité descendante uniquement. La valeur est toujours 0.|  
|**load_os_resources**|**int**|Indique si les analyseurs lexicaux, les générateurs de formes dérivées et les filtres du système d'exploitation sont inscrits et utilisés avec cette instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Une des valeurs suivantes :<br /><br /> 0 = Utiliser uniquement les filtres et les analyseurs lexicaux propres à cette instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].<br /><br /> 1 = Charger les filtres et les analyseurs lexicaux du système d'exploitation.<br /><br /> Par défaut, cette propriété est désactivée afin d'empêcher des modifications de comportement accidentelles suite à des mises à jour du système d'exploitation. L'utilisation des ressources du système d'exploitation permet d'accéder aux ressources associées aux langues et types de document inscrits avec le service d'indexation [!INCLUDE[msCoName](../../includes/msconame-md.md)] pour lesquels aucune ressource propre à l'instance n'est installée. Si vous activez le chargement des ressources du système d’exploitation, assurez-vous que les ressources de système d’exploitation sont les fichiers binaires signés approuvés ; dans le cas contraire, ils ne peuvent pas être chargées lorsque **verify_signature** (voir ci-dessous) est définie sur 1.|  
|**master_merge_dop**|**int**|Spécifie le nombre de threads à utiliser par le processus de fusion principal. Cette valeur ne doit pas dépasser le nombre d'UC ou de noyaux d'UC disponibles.<br /><br /> Si cet argument n'est pas spécifié, le service en utilise 4, ou le nombre d'UC ou de noyaux d'UC disponibles, la valeur la plus petite étant applicable.|  
|**pause_indexing**|**int**|Spécifie si l'indexation de texte intégral doit être suspendue si elle est en cours d'exécution, ou reprise si elle est actuellement suspendue.<br /><br /> 0 = Reprend les activités d'indexation de texte intégral pour l'instance de serveur.<br /><br /> 1 = Suspend les activités d'indexation de texte intégral pour l'instance de serveur.|  
|**resource_usage**|**int**|N'a aucune fonction dans [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et les versions ultérieures, et est ignoré.|  
|**update_languages**|NULL|Met à jour la liste des langues et des filtres qui sont inscrits avec la recherche en texte intégral. Les langues sont spécifiées lors de la configuration de l'indexation et dans les requêtes de texte intégral. Les filtres sont utilisés par l’hôte de démon de filtre pour extraire des informations textuelles de formats de fichiers correspondants, tels que .docx, stockées dans des types de données, tel que **varbinary**, **varbinary (max)**, **image**, ou **xml**, pour l’indexation de texte intégral.<br /><br /> Pour plus d’informations, consultez [Afficher ou modifier des filtres et des analyseurs lexicaux inscrits](../../relational-databases/search/view-or-change-registered-filters-and-word-breakers.md).|  
|**upgrade_option**|**int**|Contrôle la manière dont les index de recherche en texte intégral sont migrés lors d'une mise à niveau d'une base de données de [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] vers une version ultérieure. Cette propriété s'applique à la mise à niveau par attachement d'une base de données, restauration d'une sauvegarde de la base de données, restauration d'une sauvegarde de fichiers ou copie de la base de données à l'aide de l'Assistant Copie de base de données.<br /><br /> Une des valeurs suivantes :<br /><br /> 0 = Les catalogues de texte intégral sont reconstruits à l'aide des analyseurs lexicaux nouveaux et améliorés. La reconstruction des index peut prendre du temps, et une quantité importante de ressources en termes d'UC et de mémoire peut être requise après la mise à niveau.<br /><br /> 1 = Les catalogues de texte intégral sont réinitialisés. [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] Les fichiers de catalogue de texte intégral sont supprimés, mais les métadonnées pour les catalogues de texte intégral et les index de recherche en texte intégral sont conservés. Après leur mise à niveau, tous les index de recherche en texte intégral ont le suivi des modifications désactivé et aucune analyse n'est démarrée automatiquement. Le catalogue reste vide tant que vous n'avez pas procédé manuellement à une alimentation complète, au terme de la mise à niveau.<br /><br /> 2 = Les catalogues de texte intégral sont importés. En général, l'importation est considérablement plus rapide que lors d'une reconstruction (rebuild). Par exemple, lorsque vous utilisez un seul processeur, l'importation s'exécute approximativement 10 fois plus vite que lors de la reconstruction. Toutefois, un catalogue de texte intégral importé n'utilise pas les analyseurs lexicaux nouveaux et améliorés, ce qui fait que vous pouvez le cas échéant reconstruire vos catalogues de texte intégral au final.<br /><br /> Remarque : La reconstruction peut s’exécuter en mode multithread, et si plus de 10 processeurs sont disponibles, de reconstruction peut s’exécuter plus rapidement que l’importation si vous la laissez utiliser tous les processeurs.<br /><br /> Si aucun catalogue de texte intégral n'est disponible, les index de recherche en texte intégral associés sont reconstruits. Cette option est disponible uniquement pour les bases de données [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] .<br /><br /> Pour plus d’informations sur le choix d’une option de mise à niveau de recherche en texte intégral, consultez[Mise à niveau de la fonction de recherche en texte intégral](../../relational-databases/search/upgrade-full-text-search.md).<br /><br /> Remarque : Pour définir cette propriété dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], utilisez le **Option de mise à niveau de recherche en texte intégral** propriété. Pour plus d’informations, consultez [Gérer et surveiller la recherche en texte intégral pour une instance de serveur](../../relational-databases/search/manage-and-monitor-full-text-search-for-a-server-instance.md).|  
|**verify_signature**|**int**|Indique si seuls les binaires signés sont chargés par le moteur de recherche en texte intégral. Par défaut, seuls les binaires approuvés et signés sont chargés.<br /><br /> 1 = Vérifier que seuls les binaires signés et approuvés sont chargés (valeur par défaut).<br /><br /> 0 = Ne pas vérifier si les binaires sont signés.|  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 0 (réussite) ou 1 (échec)  
  
## <a name="result-sets"></a>Jeux de résultats  
 Aucun  
  
## <a name="permissions"></a>Autorisations  
 Seuls les membres de la **serveradmin** rôle serveur fixe ou l’administrateur système peut exécuter **sp_fulltext_service**.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-updating-the-list-of-registered-languages"></a>A. Mise à jour de la liste des langues inscrites  
 L'exemple ci-dessous met à jour la liste des langues inscrites avec la recherche en texte intégral.  
  
```  
EXEC sp_fulltext_service 'update_languages';  
GO  
```  
  
### <a name="b-changing-the-full-text-upgrade-option-to-reset-full-text-catalogs"></a>B. Modification de l'option de mise à niveau du catalogue de texte intégral pour réinitialiser des catalogues de texte intégral  
 L'exemple ci-dessous modifie l'option de mise à niveau du catalogue de texte intégral pour réinitialiser des catalogues de texte intégral. Cela les supprime complètement. Cet exemple spécifie les mots clés facultatifs `@action` et `@value`.  
  
```  
EXEC sp_fulltext_service @action='upgrade_option', @value=1;  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Recherche en texte intégral](../../relational-databases/search/full-text-search.md)   
 [FULLTEXTSERVICEPROPERTY &#40;Transact-SQL&#41;](../../t-sql/functions/fulltextserviceproperty-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
