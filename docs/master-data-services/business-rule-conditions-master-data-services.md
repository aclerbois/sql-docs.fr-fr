---
title: Conditions de règle d’entreprise (Master Data Services) | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: mds
ms.component: non-specific
ms.reviewer: ''
ms.suite: sql
ms.technology:
- master-data-services
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: d2e0a8c3-4c2e-407c-856e-68d95ebda9ed
caps.latest.revision: 10
author: leolimsft
ms.author: lle
manager: craigg
ms.openlocfilehash: 328aa233b750404e28984dec5d93bb7070a0bcc6
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="business-rule-conditions-master-data-services"></a>Conditions de règle d'entreprise (Master Data Services)

[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

  Dans [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)], les conditions de règle d'entreprise déterminent les conditions qui doivent être réunies pour qu'une ou plusieurs actions soient entreprises.  
  
> [!NOTE]  
>  Les conditions sont facultatives. Si vous ne spécifiez pas de condition, les actions sont entreprises à chaque fois que les données sont validées par rapport aux règles d'entreprise.  
  
## <a name="business-rule-conditions"></a>Conditions de règle d'entreprise  
  
|Nom de la condition|Description|  
|--------------------|-----------------|  
|**est égal à**|L'attribut sélectionné **est égal à** un attribut spécifique, une valeur d'attribut spécifique, ou est vide.<br /><br /> Cette condition est valide pour les valeurs de texte, nombre, date et lien.|  
|**n'est pas égal à**|L'attribut sélectionné **n'est pas égal à** un attribut spécifique, une valeur d'attribut spécifique, ou est vide.<br /><br /> Cette condition est valide pour les valeurs de texte, nombre, date et lien.|  
|**est supérieur à**|L'attribut sélectionné **est supérieur à** un attribut spécifique, une valeur d'attribut spécifique, ou est vide.<br /><br /> Cette condition est valide pour les valeurs de texte, nombre et date.|  
|**est supérieur ou égal à**|L'attribut sélectionné **est supérieur ou égal à** un attribut spécifique, une valeur d'attribut spécifique, ou est vide.<br /><br /> Cette condition est valide pour les valeurs de texte, nombre et date.|  
|**est inférieur à**|L'attribut sélectionné **est inférieur à** un attribut spécifique, une valeur d'attribut spécifique, ou est vide.<br /><br /> Cette condition est valide pour les valeurs de texte, nombre et date.|  
|**est inférieur ou égal à**|L'attribut sélectionné **est inférieur ou égal à** un attribut spécifique, une valeur d'attribut spécifique, ou est vide.<br /><br /> Cette condition est valide pour les valeurs de texte, nombre et date.|  
|**commence par**|L'attribut sélectionné **commence par** un attribut spécifique, une valeur d'attribut spécifique, ou est vide.<br /><br /> Cette condition est valide pour les valeurs de texte et de lien.|  
|**ne commence pas par**|L’attribut sélectionné **ne commence pas par** un attribut spécifique, une valeur d’attribut spécifique, ou est vide.<br /><br /> Cette condition est valide pour les valeurs de texte et de lien.|  
|**se termine par**|L'attribut sélectionné **se termine par** un attribut spécifique, une valeur d'attribut spécifique, ou est vide.<br /><br /> Cette condition est valide pour les valeurs de texte et de lien.|  
|**ne se termine pas par**|L’attribut sélectionné **ne se termine pas par** un attribut spécifique, une valeur d’attribut spécifique, ou est vide.<br /><br /> Cette condition est valide pour les valeurs de texte et de lien.|  
|**contient**|L'attribut sélectionné **contient** un attribut spécifique, une valeur d'attribut spécifique, ou est vide.<br /><br /> Cette condition est valide pour les valeurs de texte et de lien.|  
|**ne contient pas**|L’attribut sélectionné **ne contient pas** un attribut spécifique, une valeur d’attribut spécifique, ou est vide.<br /><br /> Cette condition est valide pour les valeurs de texte et de lien.|  
|**contient le modèle**|L'attribut sélectionné **contient le modèle** d'un attribut spécifique, d'une valeur d'attribut spécifique, ou est vide. Utilisez des expressions régulières .NET Framework pour spécifier le modèle.<br /><br /> Pour plus d'informations sur les expressions régulières, consultez [Éléments du langage des expressions régulières](http://go.microsoft.com/fwlink/?LinkId=164401) dans MSDN Library.<br /><br /> Cette condition est valide pour les valeurs de texte et de lien.|  
|**ne contient pas le modèle**|L’attribut sélectionné **ne contient pas le modèle** d’un attribut spécifique, une valeur d’attribut spécifique, ou est vide. Utilisez des expressions régulières .NET Framework pour spécifier le modèle.<br /><br /> Pour plus d'informations sur les expressions régulières, consultez [Éléments du langage des expressions régulières](http://go.microsoft.com/fwlink/?LinkId=164401) dans MSDN Library.<br /><br /> Cette condition est valide pour les valeurs de texte et de lien.|  
|**contient le sous-ensemble**|L'attribut sélectionné **contient le sous-ensemble** d'un attribut spécifique ou d'une valeur d'attribut spécifique. Vous devez spécifier la position de départ pour la recherche (par exemple, 1 signifie que la recherche débute au premier caractère).<br /><br /> Cette condition est valide pour les valeurs de texte et de lien.|  
|**ne contient pas le sous-ensemble**|L’attribut sélectionné **ne contient pas le sous-ensemble** d’un attribut spécifique ou d’une valeur d’attribut spécifique. Vous devez spécifier la position de départ pour la recherche (par exemple, 1 signifie que la recherche débute au premier caractère).<br /><br /> Cette condition est valide pour les valeurs de texte et de lien.|  
|**a changé**|L'attribut sélectionné **a changé** depuis la dernière application des règles d'entreprise au membre. Vous devez spécifier le groupe de modification dont l'attribut est membre.<br /><br /> Pour plus d’informations sur les groupes de suivi des modifications, consultez [Ajouter des attributs à un groupe de suivi des modifications &#40;Master Data Services&#41;](../master-data-services/add-attributes-to-a-change-tracking-group-master-data-services.md).<br /><br /> Cette condition est valide pour les valeurs de texte, nombre, date et lien.|  
|**n’a pas changé**|L’attribut sélectionné **n’a pas changé** depuis la dernière application des règles d’entreprise au membre. Vous devez spécifier le groupe de modification dont l'attribut est membre.<br /><br /> Pour plus d’informations sur les groupes de suivi des modifications, consultez [Ajouter des attributs à un groupe de suivi des modifications &#40;Master Data Services&#41;](../master-data-services/add-attributes-to-a-change-tracking-group-master-data-services.md).<br /><br /> Cette condition est valide pour les valeurs de texte, nombre, date et lien.|  
|**est compris entre**|L'attribut sélectionné **est compris entre** deux valeurs d'attribut spécifiques.<br /><br /> Cette condition est valide pour les valeurs de texte, nombre et date.|  
|**n’est pas compris entre**|L’attribut sélectionné **n’est pas compris entre** deux valeurs d’attribut spécifiques.<br /><br /> Cette condition est valide pour les valeurs de texte, nombre et date.|  
  
> [!NOTE]  
>  Lorsqu'une règle d'entreprise contenant une condition qui compare deux valeurs est appliquée à un membre pour lequel les deux valeurs sont NULL, la validation de ce membre échoue.  
  
## <a name="see-also"></a> Voir aussi  
 [Actions de règle d’entreprise &#40;Master Data Services&#41;](../master-data-services/business-rule-actions-master-data-services.md)   
 [Règles d’entreprise &#40;Master Data Services&#41;](../master-data-services/business-rules-master-data-services.md)   
 [Créer et publier une règle d’entreprise &#40;Master Data Services&#41;](../master-data-services/create-and-publish-a-business-rule-master-data-services.md)  
  
  
