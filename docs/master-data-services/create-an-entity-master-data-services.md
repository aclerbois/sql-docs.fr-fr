---
title: Créer une entité (Master Data Services) | Microsoft Docs
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
helpviewer_keywords:
- entities [Master Data Services], creating
- creating entities [Master Data Services]
ms.assetid: d9a6a51e-7b53-4785-a118-3baeb7ca2d48
caps.latest.revision: 9
author: leolimsft
ms.author: lle
manager: craigg
ms.openlocfilehash: e2e9851921d808339681080d5df0c9439212a3db
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="create-an-entity-master-data-services"></a>Créer une entité (Master Data Services)

[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

  Dans [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)], créez une entité destinée à contenir des membres et leurs attributs.  
  
## <a name="prerequisites"></a>Conditions préalables requises  
 Pour effectuer cette procédure :  
  
-   Vous devez avoir l'autorisation d'accéder à la zone fonctionnelle **Administration de système** .  
  
-   Vous devez être administrateur de modèle. Pour plus d’informations, consultez [Administrateurs &#40;Master Data Services&#41;](../master-data-services/administrators-master-data-services.md).  
  
-   Un modèle doit exister. Pour plus d’informations, consultez [Créer un modèle &#40;Master Data Services&#41;](../master-data-services/create-a-model-master-data-services.md).  
  
### <a name="to-create-an-entity"></a>Pour créer une entité  
  
1.  Dans [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)], cliquez sur **Administration de système**.  
  
2.  Dans la grille de la page **Gérer les modèles** , sélectionnez le modèle pour lequel vous souhaitez créer une entité, puis cliquez sur **Entités**.  
  
3.  Dans la page **Gérer les entités** , cliquez sur **Ajouter**.  
  
4.  Dans la zone **Nom** , tapez le nom de l’entité.  
  
5.  Si vous le souhaitez, dans le champ **Description** , tapez la description de l’entité.  
  
6.  Dans la zone **Nom des tables intermédiaires** , tapez éventuellement un nom pour la table de mise en lots.  
  
     Si vous ne renseignez pas ce champ, le nom de l’entité sera utilisé.  
  
    > [!TIP]  
    >  Utilisez le nom du modèle dans le nom de la table de mise en lots, par exemple *NomModèle_NomEntité*. Cela facilite la recherche de tables dans la base de données. Pour plus d’informations sur les tables de mise en lots, consultez [Présentation : Importation de données à partir de tables &#40;Master Data Services&#41;](../master-data-services/overview-importing-data-from-tables-master-data-services.md).  
  
7.  Pour le champ **Type de journal des transactions**, choisissez le type du journal des transactions dans la liste déroulante.  
  
     Pour plus d’informations, consultez [Modifier le type du journal des transactions de l’entité &#40;Master Data Services&#41;](../master-data-services/change-the-entity-transaction-log-type-master-data-services.md).  
  
8.  Facultatif. Activez la case à cocher **Créer automatiquement les valeurs de code** . Pour plus d’informations, consultez [Création automatique de code &#40;Master Data Services&#41;](../master-data-services/automatic-code-creation-master-data-services.md).  
  
9. Facultatif. Cochez la case **Activer la compression des données** . La compression de ligne est activée par défaut. Pour plus d’informations, voir [Data Compression](../relational-databases/data-compression/data-compression.md).  
  
10. Cliquez sur **Enregistrer**.  
  
## <a name="grid-columns"></a>Colonnes de la grille  
 Pour chaque entité créée, une ligne comportant treize colonnes est ajoutée à la grille. Les différentes colonnes sont décrites ci-après.  
  
|Nom   |Description|  
|----------|-----------------|  
|État|État de l’entité. Lorsque vous cliquez sur **Enregistrer** , l’image ci-après s’affiche pour indiquer que l’entité est en cours de mise à jour.<br /><br /> ![Icône d’état de mise à jour](../master-data-services/media/mds-statusicon-updating.png "Icône d’état de mise à jour")<br /><br /> En cas d’erreur lors de la création ou de la modification d’une entité, l’image suivante apparaît.<br /><br /> ![Icône d’état d’erreur](../master-data-services/media/mds-statusicon-error.png "Icône d’état d’erreur")<br /><br /> Si l’état présente la valeur OK, l’image ci-dessous s’affiche.<br /><br /> ![Icône d’état OK](../master-data-services/media/mds-statusicon-ok.png "Icône d’état OK")|  
|Nom   |Nom de l’entité.|  
|Description|Description de l’entité.|  
|Table de mise en lots|Nom de préfixe de la table utilisée pour le stockage des données.|  
|Type de journal des transactions|Type du journal des transactions de l’entité.|  
|Création automatique de code|Indique si la création automatique de code est activée.|  
|Data Compression|Indique si la compression des données est activée pour l’entité.|  
|Cible de synchronisation|Indique si l’entité est la cible d’une relation de synchronisation.|  
|Activée pour les hiérarchies|Indique si l’entité est activée pour les hiérarchies explicites. Cette colonne affiche la valeur Oui si au moins une hiérarchie explicite est créée pour l’entité.|  
|Date de création|Nom de l’utilisateur ayant créé l’entité.|  
|Créée le|Date et heure de création de l’entité.|  
|Mise à jour par|Nom de l’utilisateur ayant effectué la dernière mise à jour de l’entité.|  
|Mise à jour le|Date et heure de la dernière mise à jour de l’entité.|  
  
## <a name="next-steps"></a>Next Steps  
  
-   [Créer un attribut de texte &#40;Master Data Services&#41;](../master-data-services/create-a-text-attribute-master-data-services.md)  
  
-   [Créer un attribut basé sur un domaine &#40;Master Data Services&#41;](../master-data-services/create-a-domain-based-attribute-master-data-services.md)  
  
-   [Créer un attribut de fichier &#40;Master Data Services&#41;](../master-data-services/create-a-file-attribute-master-data-services.md)  
  
## <a name="see-also"></a> Voir aussi  
 [Entités &#40;Master Data Services&#41;](../master-data-services/entities-master-data-services.md)   
 [Hiérarchies explicites &#40;Master Data Services&#41;](../master-data-services/explicit-hierarchies-master-data-services.md)   
 [Modifier une entité &#40;Master Data Services&#41;](../master-data-services/edit-an-entity-master-data-services.md)   
 [Supprimer une entité &#40;Master Data Services&#41;](../master-data-services/delete-an-entity-master-data-services.md)  
  
  
