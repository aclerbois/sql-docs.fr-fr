---
title: Références à des collections DataSources et DataSets (Générateur de rapports et SSRS) | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-sharepoint, reporting-services-native
ms.component: report-design
ms.reviewer: ''
ms.suite: pro-bi
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: f951a4aa-da55-4e43-8579-4a5d4480d11f
caps.latest.revision: 7
author: maggiesMSFT
ms.author: maggies
manager: kfile
ms.openlocfilehash: b6e03491b893ac6c77a202207ff7e296ac20f153
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="built-in-collections---datasources-and-datasets-references-report-builder"></a>Collections intégrées - Références à des collections DataSources et DataSets (Générateur de rapports)
  La collection **DataSources** représente toutes les sources de données utilisées dans un rapport. De la même façon, la collection **DataSets** représente tous les datasets de toutes les sources de données dans un rapport. Utilisez le volet **Données du rapport** pour une vue hiérarchique des datasets du rapport organisés sous la source de données à laquelle ils font référence. Si vous incluez des références à ces collections, vous ne verrez pas de valeurs lors de l'aperçu de votre rapport. Ces collections sont disponibles uniquement après la publication du rapport sur un serveur de rapports.  
  
> [!NOTE]  
>  [!INCLUDE[ssRBRDDup](../../includes/ssrbrddup-md.md)]  
  
## <a name="datasources"></a>DataSources  
 La collection **DataSources** représente les sources de données référencées dans la définition d’un rapport publié. Vous pouvez choisir d'inclure ces informations dans votre rapport pour documenter la source des données de rapport. Cette collection n’est pas disponible en mode **Aperçu** . Le tableau ci-dessous décrit les variables de la collection **DataSources** .  
  
|**Variable**|**Type**|**Description**|  
|------------------|--------------|---------------------|  
|**DataSourceReference**|**String**|Chemin d'accès complet de la définition de source de données sur le serveur de rapports. Par exemple, vous pouvez inclure une liste de toutes les sources de données qu'un rapport a utilisées dans le cadre d'un historique de rapport. L'exemple suivant affiche le chemin d'accès complet de la source de données nommée AdventureWorks2012 :<br /><br /> `/DataSources/AdventureWorks2012`.|  
|**Type**|**String**|Type du fournisseur de données pour la source de données. Par exemple, `SQL`.|  
  
## <a name="datasets"></a>DataSets  
 La collection **DataSets** représente les datasets référencés dans une définition de rapport. Vous pouvez choisir d'inclure la requête du rapport dans une zone de texte afin qu'un utilisateur intéressé par le contenu exact du rapport puisse examiner le texte de la commande d'origine. Cette collection n’est pas disponible en mode **Aperçu** . Le tableau ci-dessous décrit les membres de la collection **DataSets** .  
  
|**Membre**|**Type**|**Description**|  
|----------------|--------------|---------------------|  
|**CommandText**|**String**|Pour les sources de données de base de données, il s'agit de la requête employée pour récupérer des données à partir de la source de données. Si la requête est une expression, elle correspond à l'expression évaluée.|  
|**RewrittenCommandText**|**String**|Valeur CommandText développée du fournisseur de données. Elle est généralement utilisée pour les rapports dont les paramètres de requête sont mappés aux paramètres du rapport. Le fournisseur de données définit cette propriété lors du développement des références de paramètre du texte de commande dans les valeurs de constante sélectionnées pour les paramètres de rapport mappés.|  
  
### <a name="using-query-expressions"></a>Utilisation d'expressions de requête  
 Vous pouvez utiliser des expressions pour définir la requête contenue dans un dataset. Vous pouvez utiliser cette fonctionnalité pour concevoir des rapports où la requête change en fonction de l'entrée de l'utilisateur, de données d'autres datasets ou d'autres variables. Pour plus d’informations sur les requêtes, consultez [Datasets incorporés dans les rapports et datasets partagés &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-data/report-embedded-datasets-and-shared-datasets-report-builder-and-ssrs.md).  
  
## <a name="see-also"></a> Voir aussi  
 [Expressions &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/expressions-report-builder-and-ssrs.md)   
 [Exemples d’expressions &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/expression-examples-report-builder-and-ssrs.md)  
  
  
