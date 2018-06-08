---
title: Créer un attribut de date (Master Data Services) | Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
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
- creating date attributes [Master Data Services]
- attributes [Master Data Services], creating date attributes
ms.assetid: 22a8f1a3-b4f2-4cfa-8495-7daad5ce9d12
caps.latest.revision: 13
author: leolimsft
ms.author: lle
manager: craigg
ms.openlocfilehash: 16ff8c3e424cfc7d82071c0378b0d2ce0fdd4ade
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="create-a-date-attribute-master-data-services"></a>Créer un attribut de date (Master Data Services)

[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

  Dans [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)], vous pouvez créer un attribut date lorsque vous souhaitez que les utilisateurs entrent une date comme valeur d'attribut.  
  
> [!NOTE]  
>  L'attribut est appelée DateTime, mais les valeurs d'heure ne sont pas prises en charge.  
  
## <a name="prerequisites"></a>Conditions préalables requises  
 Pour effectuer cette procédure :  
  
-   Vous devez avoir l'autorisation d'accéder à la zone fonctionnelle **Administration de système** .  
  
-   Vous devez être administrateur de modèle. Pour plus d’informations, consultez [Administrateurs &#40;Master Data Services&#41;](../master-data-services/administrators-master-data-services.md).  
  
-   Vous devez avoir une entité pour laquelle créer l'attribut. Pour plus d’informations, consultez [Créer une entité &#40;Master Data Services&#41;](../master-data-services/create-an-entity-master-data-services.md).  
  
### <a name="to-create-a-date-attribute"></a>Pour créer un attribut de date  
  
1.  Dans [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)], cliquez sur **Administration de système**.  
  
2.  Dans la page **Gérer le modèle** , sélectionnez un modèle dans la grille et cliquez sur **Entités**.  
  
3.  Dans la page **Gérer l’entité** , sélectionnez la ligne de l’entité pour laquelle vous souhaitez créer un attribut.  
  
4.  Cliquez sur **Attributs**.  
  
5.  Dans la page **Gérer les attributs** , effectuez l’une des opérations suivantes, puis cliquez sur **Ajouter**.  
  
    -   Si l’attribut concerne les membres feuille, sélectionnez **Feuille** dans la zone de liste **Types de membre** .  
  
    -   Si l’attribut concerne les membres consolidés, sélectionnez **Consolidé** dans la zone de liste **Types de membre** .  
  
    -   Si l’attribut concerne les collections, sélectionnez **Collection** dans la zone de liste **Types de membre** .  
  
6.  Dans la zone **Nom** , tapez un nom pour l'attribut. Pour obtenir la liste des mots qui ne doivent pas être utilisés comme noms d’attributs, consultez [Mots réservés &#40;Master Data Services&#41;](../master-data-services/reserved-words-master-data-services.md).  
  
7.  Si vous le souhaitez, tapez le nom complet et une description de l’attribut dans le champ **Description** .  
  
8.  Dans la zone **Largeur d'affichage en pixels** , tapez la largeur de la colonne d'attribut à afficher dans la grille **Explorateur** .  
  
9. Dans la liste **Type d’attribut** , sélectionnez **Forme libre**.  
  
10. Dans la liste **Type de données** , sélectionnez **DateTime**.  
  
11. Dans la liste **Masque d’entrée** , sélectionnez un format pour les dates.  
  
12. Sélectionnez éventuellement **Activer le suivi des modifications** pour effectuer le suivi des modifications apportées aux groupes d'attributs. Pour plus d’informations, consultez [Ajouter des attributs à un groupe de suivi des modifications &#40;Master Data Services&#41;](../master-data-services/add-attributes-to-a-change-tracking-group-master-data-services.md).  
  
13. Cliquez sur **Enregistrer**.  
  
## <a name="to-display-the-time-portion-of-a-datetime-value"></a>Pour afficher la partie heure d'une valeur datetime  
 Pour que l'interface utilisateur affiche la partie heure d'une valeur datetime, vous devez sélectionner un masque de saisie approprié pour l'attribut. Aucun des masques intégrés des attributs Datetime ne le permet, mais vous avez la possibilité d'ajouter un nouveau masque grâce auquel vous pourrez afficher l'heure. Pour ce faire, ajoutez une ligne dans la table mdm.tblList de la base de données MDS, dans laquelle sont stockés les masques intégrés. La ligne doit contenir les valeurs suivantes :  
  
|||  
|-|-|  
|ListCode|lstInputMask|  
|ListName|Masque d’entrée|  
|Seq|19|  
|Option de liste|jj/MM/aaaa hh:mm:ss tt|  
|ID d'option|19|  
|EstVisible| 1|  
|Group_ID|3|  
  
 Une fois que vous avez entré une ligne comportant les valeurs ci-dessus dans la table mdm.tblList, le masque « jj/MMM/aaaa hh:mm:ss tt » devient disponible dans la zone de liste de masque de saisie. Vous pouvez alors sélectionner ce masque afin d'afficher la date et l'heure dans la colonne d'attribut datetime d'une entité dans l'Explorateur MDS.  
  
 Le masque de saisie est une chaîne au format DateTime .NET personnalisée. Pour plus d’informations, consultez [Chaînes de format de date et d’heure personnalisées](https://msdn.microsoft.com/en-us/library/8kb3ddd4\(v=vs.110\).aspx).  
  
## <a name="see-also"></a> Voir aussi  
 [Attributs &#40;Master Data Services&#41;](../master-data-services/attributes-master-data-services.md)   
 [Changer le nom d’un attribut et un type de données &#40;Master Data Services&#41;](../master-data-services/change-an-attribute-name-and-data-type-master-data-services.md)   
 [Créer un attribut basé sur un domaine &#40;Master Data Services&#41;](../master-data-services/create-a-domain-based-attribute-master-data-services.md)   
 [Créer un attribut de fichier &#40;Master Data Services&#41;](../master-data-services/create-a-file-attribute-master-data-services.md)  
  
  
