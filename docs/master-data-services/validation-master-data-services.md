---
title: Validation (Master Data Services) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: mds
ms.component: non-specific
ms.reviewer: ''
ms.suite: sql
ms.technology:
- master-data-services
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: 98eb49e7-b190-4a21-8316-08c07cde14ed
caps.latest.revision: 12
author: leolimsft
ms.author: lle
manager: craigg
ms.openlocfilehash: 64afad9735943432a3b310d82a561ee35880bb42
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="validation-master-data-services"></a>Validation (Master Data Services)

[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

  Dans [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)], les données sont validées pour en garantir l'exactitude. Une partie de la validation s'effectue automatiquement, tandis qu'une autre partie est basée sur des règles d'entreprise créées par les administrateurs.  
  
## <a name="when-data-validation-occurs"></a>Occurrences de la validation des données  
 La validation se produit à différents moments et s'affiche de différentes manières dans l'application Web [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] .  
  
|Type de validation|Standards déterminés par|Lorsqu'elle se produit|Affichée dans l'interface utilisateur Web du gestionnaire de MasterData en tant que|Affichée dans le complément pour Excel en tant que|Les données sont-elles enregistrées dans le référentiel MDS ?|  
|---------------------|-----------------------------|--------------------|---------------------------------------------------|-------------------------------------------|------------------------------------------|  
|Validation de la règle d'entreprise|Administrateur MDS|Automatiquement lorsqu'un utilisateur ajoute ou modifie des données.<br /><br /> Manuellement lorsqu'un utilisateur applique des règles d'entreprise.<br /><br /> Manuellement, lorsqu'un administrateur dans la zone fonctionnelle **Gestion des versions** de l'application Web [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)] valide une version par rapport aux règles d'entreprise.|Erreurs de validation|ValidationStatus|Oui|  
|Validation de type de données et de contenu|Un administrateur MDS, en créant des objets de modèle (par exemple, la longueur ou le type de données d'un attribut)|Automatiquement lorsqu'un utilisateur ajoute ou modifie des données.|Erreurs d'entrée|InputStatus|non|  
|Validation de type de données et de contenu|[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] ou [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)]|Automatiquement lorsqu'un utilisateur ajoute ou modifie des données.|Erreurs d'entrée|InputStatus|non|  
  
## <a name="related-tasks"></a>Related Tasks  
  
|Description de la tâche|Rubrique|  
|----------------------|-----------|  
|Créer et publier des règles d'entreprise par rapport auxquelles valider des données.|[Créer et publier une règle d’entreprise &#40;Master Data Services&#41;](../master-data-services/create-and-publish-a-business-rule-master-data-services.md)|  
|Valider une version des données par rapport aux règles d'entreprise. Administrateurs uniquement.|[Valider une version par rapport aux règles d’entreprise &#40;Master Data Services&#41;](../master-data-services/validate-a-version-against-business-rules-master-data-services.md)|  
|Valider des sous-ensembles de données spécifiques par rapport aux règles d'entreprise. Tous les utilisateurs autorisés à accéder à la zone fonctionnelle **Explorateur** .|[Valider des membres spécifiques par rapport aux règles d’entreprise &#40;Master Data Services&#41;](../master-data-services/validate-specific-members-against-business-rules-master-data-services.md)|  
|Valider des sous-ensembles de données spécifiques par rapport aux règles d'entreprise. Tous les utilisateurs autorisés à accéder à la zone fonctionnelle **Explorateur** et utilisant [!INCLUDE[ssMDSXLS](../includes/ssmdsxls-md.md)].|[Appliquer des règles d’entreprise &#40;Complément MDS pour Excel&#41;](../master-data-services/microsoft-excel-add-in/apply-business-rules-mds-add-in-for-excel.md)|  
  
## <a name="see-also"></a> Voir aussi  
 [Business Rules &#40;Master Data Services&#41;](../master-data-services/business-rules-master-data-services.md)  
  
  
