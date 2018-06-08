---
title: Opérateurs de filtre (Master Data Services) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: mds
ms.component: non-specific
ms.reviewer: ''
ms.suite: sql
ms.technology:
- master-data-services
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: 27914c8b-8951-4b7d-914d-1cbf528dd248
caps.latest.revision: 11
author: leolimsft
ms.author: lle
manager: craigg
ms.openlocfilehash: f4d3785155a0027f374b25f22b9586a215fd4ae9
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="filter-operators-master-data-services"></a>Opérateurs de filtre (Master Data Services)

[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

  Lors du filtrage d'une liste de membres, les opérateurs suivants sont disponibles.  
  
> [!NOTE]  
>  Lorsque vous filtrez selon plusieurs critères, tous les critères doivent être remplis pour que des résultats soient retournés. Par exemple, SquareFeet = 2000 **AND** Division <> 123.  
  
## <a name="filter-operators"></a>Opérateurs de filtre  
  
|Nom du contrôle|Description|  
|------------------|-----------------|  
|**Est égal à**|Retourne des valeurs d'attribut qui sont exactement les mêmes que les critères spécifiés. Par exemple, vous devez taper **Mountain-100**pour filtrer sur **Mountain-100**.|  
|**n'est pas égal à**|Retourne des valeurs d'attribut qui ne sont pas exactement les mêmes que les critères spécifiés. Les critères de filtre doivent être exactement les mêmes que la valeur d'attribut que vous voulez omettre des résultats. Par exemple, pour omettre les résultats qui correspondent à **Mountain-100**, vous devez taper **Mountain-100**.<br /><br /> <br /><br /> Remarque : lorsque vous appliquez une condition de filtre avec une clause « N’est pas égal à » sur un attribut, un membre pour lequel l’attribut est NULL passe la condition de filtre et est retourné si SET ANSI_NULLS a la valeur ON dans vos paramètres de base de données. Pour désactiver ce comportement, attribuez à SET ANSI_NULLS la valeur OFF dans vos paramètres de base de données. Lorsque SET ANSI_NULLS a la valeur OFF, les comparaisons de toutes les données par rapport à une valeur NULL sont évaluées à TRUE si la valeur des données est NULL, et par conséquent, le membre ne passe pas la clause « N'est pas égal à ». Pour plus d’informations, consultez [SET ANSI_NULLS &#40;Transact-SQL&#41;](../t-sql/statements/set-ansi-nulls-transact-sql.md).|  
|**Est identique à**|Utilise l'opérateur LIKE de Transact-SQL pour filtrer les résultats. Pour plus d’informations, consultez [LIKE &#40;Transact-SQL&#41;](../t-sql/language-elements/like-transact-sql.md) dans la documentation en ligne de SQL Server.|  
|**est différent de**|Utilise l'opérateur NOT de Transact-SQL pour filtrer les résultats. Pour plus d’informations, consultez [NOT &#40;Transact-SQL&#41;](../t-sql/language-elements/not-transact-sql.md) dans la documentation en ligne de SQL Server.|  
|**Est supérieur à**|Retourne des valeurs d'attribut qui sont supérieures aux critères spécifiés. Par exemple, pour retourner des valeurs d’attribut qui commencent par une lettre supérieure à **F**, tapez **F**.|  
|**Est inférieur à**|Retourne des valeurs d'attribut qui sont inférieures aux critères spécifiés. Par exemple, pour retourner des valeurs d’attribut qui commencent par une lettre inférieure à **F**, tapez **F**.|  
|**Est supérieur ou égal à**|Retourne des valeurs d'attribut qui sont supérieures ou égales aux critères spécifiés. Par exemple, pour retourner des valeurs d’attribut qui démarrent par le chiffre **3** ou un chiffre supérieur, tapez **3**.|  
|**Est inférieur ou égal à**|Retourne des valeurs d'attribut qui sont inférieures ou égales aux critères spécifiés. Par exemple, pour retourner des valeurs d’attribut qui démarrent par le chiffre **3** ou un chiffre inférieur, tapez **3**.|  
|**Correspondances**|Utilise un index de recherche floue pour filtrer les résultats.<br /><br /> Utilisez le champ **Niveau de similarité** pour spécifier dans quelle mesure les valeurs d’attribut doivent correspondre aux critères de filtre spécifiés (avec une valeur par défaut de 30 %).<br /><br /> Sélectionnez une des valeurs suivantes dans la liste déroulante **Algorithme** :<br /><br /> **Levenshtein**: distance basée sur le nombre de modifications (par exemple, ajouts ou suppressions) nécessaires pour qu’une chaîne corresponde à une autre. Il s'agit du paramètre par défaut. Ne requiert aucun paramètre supplémentaire.<br /><br /> **Jaccard**: index qui fonctionne de manière optimale lorsque vous essayez de mettre plusieurs chaînes en correspondance. Cette recherche prend en charge un paramètre supplémentaire de relation contenant-contenu (voir ci-dessous).<br /><br /> **Jaro-Winkler**: distance qui convient le mieux pour rechercher des noms de personnes en double. Cette méthode retourne plus de résultats que toute autre méthode. Ne prend pas en charge la relation contenant-contenu.<br /><br /> **Sous-séquence commune la plus longue**: repose sur une sous-séquence dans laquelle les lettres d’un modèle s’affichent dans l’ordre, même si elles peuvent être séparées (par exemple, « MSR » est une sous-séquence de « master ». Cette recherche prend en charge un paramètre supplémentaire de relation contenant-contenu (voir ci-dessous).<br /><br /> <br /><br /> Remarque : pour les algorithmes **Jaccard** ou **Sous-séquence commune la plus longue** , ajoutez une **Valeur de relation contenant-contenu**. Il s'agit d'un seuil de longueur fourni sous forme de pourcentage décimal compris entre 0 et 1, avec une valeur par défaut de 0,62. Un seuil inférieur augmente le nombre de correspondances possibles retournées.|  
|**Ne correspond pas**|Utilise un index de recherche floue pour filtrer les résultats. Utilisez le champ **Niveau de similarité** pour spécifier le niveau de précision de non correspondance des valeurs d’attribut par rapport aux critères de filtre spécifiés.|  
|**Contient le modèle**|Utilise des expressions régulières .NET Framework pour filtrer les résultats selon un modèle spécifié. Pour plus d'informations sur les expressions régulières, consultez [Éléments du langage des expressions régulières](http://go.microsoft.com/fwlink/?LinkId=164401) dans MSDN Library.|  
|**Ne contient pas le modèle**|Utilise les expressions régulières .NET Framework pour filtrer des résultats qui ne correspondent pas à un modèle spécifié. Pour plus d'informations sur les expressions régulières, consultez [Éléments du langage des expressions régulières](http://go.microsoft.com/fwlink/?LinkId=164401) dans MSDN Library.|  
|**Est NULL**|Retourne des valeurs d'attribut qui sont Null. Le champ **Critères** est désactivé lorsque vous sélectionnez l’opérateur **Est NULL** .|  
|**N'est pas NULL**|Retourne des valeurs d'attribut qui ne sont pas null. Le champ **Critères** est désactivé lorsque vous sélectionnez l’opérateur **N’est pas NULL** .|  
  
  
