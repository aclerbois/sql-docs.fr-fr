---
title: Extension de règles d’entreprise (Master Data Services) | Microsoft Docs
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
ms.assetid: 4c18be5f-a3fa-45a8-9be6-0f45f58bbc9e
caps.latest.revision: 16
author: leolimsft
ms.author: lle
manager: craigg
ms.openlocfilehash: e53c611dcbffab854e9b022389afbd3cd4463e66
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="business-rules-extension-master-data-services"></a>Extension de règles d’entreprise (Master Data Services)

[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

  Dans [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)], vous pouvez appliquer des scripts SQL définis par l’utilisateur en tant qu’extension de conditions et d’actions prédéfinies.  
  
> [!NOTE]  
>  Tous les scripts doivent être définis dans le schéma [usr].  
  
 Les fonctions SQL qui répondent aux critères suivants peuvent être utilisées comme une condition Règle d’entreprise.  
  
-   La valeur de retour doit être de type BIT.  
  
-   Seuls les types suivants sont pris en charge pour les types de paramètre.  
  
    -   NVARCHAR  
  
    -   DATETIME2  
  
    -   DECIMAL (précision, échelle)  
  
         la précision doit être 38  
  
         l’échelle doit être comprise entre 0 et 7  
  
 les procédures stockées SQL qui respectent la syntaxe suivante peuvent être utilisées comme une action Règle d’entreprise  
  
```  
CREATE PROCEDURE [usr].[YourAction]  
       (         
            @MemberIdList mdm.[MemberId] READONLY,  
            @ModelName NVARCHAR(MAX),  
            @VersionName NVARCHAR(MAX),  
            @EntityName NVARCHAR(MAX),  
            @BusinessRuleName NVARCHAR(MAX)  
        )    
      AS BEGIN    
       ...     
       END  
  
```  
  
 Les scripts définis par l’utilisateur ne sont pas ajoutés aux packages de déploiement. Vérifiez que la base de données Master Data Services cible contient tous les scripts utilisés dans les règles d’entreprise avant de déployer un package.  
  
 Les actions de script seront exécutées en tant que mds_br_user qui possède les autorisations suivantes  
  
|||  
|-|-|  
|**Schéma**|**Permissions**|  
|mdm|SELECT|  
|stg.|SELECT, UPDATE, DELETE, EXECUTE, INSERT|  
|usr|FULL|  
  
## <a name="prerequisites"></a>Conditions préalables requises  
 Pour effectuer cette procédure :  
  
-   Vous devez avoir l’autorisation d’accéder à la zone fonctionnelle Administration de système.  
  
-   Vous devez être administrateur de modèle. Pour plus d’informations, consultez [Administrateurs &#40;Master Data Services&#41;](../master-data-services/administrators-master-data-services.md).  
  
-   Des scripts définis par l’utilisateur avaient été ajoutés à la base de données [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] .  
  
## <a name="create-a-business-rule-to-take-a-user-defined-script-as-a-condition-or-as-an-action"></a>Créer une règle d’entreprise pour traiter un script défini par l’utilisateur comme une condition ou une action  
  
1.  Dans Master Data Manager, cliquez sur **Administration de système**.  
  
2.  Dans la barre de menus, pointez sur **Gérer** et cliquez sur **Règles d’entreprise**.  
  
3.  Dans la page **Règles d’entreprise** , sélectionnez un modèle dans la liste déroulante **Modèle** .  
  
4.  Dans la liste déroulante **Entité** , sélectionnez une entité.  
  
5.  Dans la liste **Types de membres** , sélectionnez un type de membre auquel appliquer la règle d’entreprise.  
  
6.  Cliquez sur **Ajouter**.  
  
7.  Procédez comme suit pour créer un script défini par l’utilisateur en tant que condition.  
  
    1.  Sous le bloc **Si** , cliquez sur le bouton **Ajouter** . Un panneau s’affiche.  
  
    2.  Dans la liste déroulante **Opérateur** , sélectionnez la fonction définie par l’utilisateur sous **Script défini par l’utilisateur** .  
  
    3.  Tous les paramètres de la fonction définie par l’utilisateur sont affichés.  
  
    4.  Affecter une valeur à chaque paramètre  
  
    5.  Cliquez sur **Enregistrer**.  
  
8.  Procédez comme suit pour créer un script défini par l’utilisateur en tant qu’action.  
  
    1.  Sous le bloc **Alors** , cliquez sur le bouton **Ajouter** . Un panneau s’affiche.  
  
    2.  Dans la liste déroulante **Opérateur** , sélectionnez la fonction définie par l’utilisateur sous **Script défini par l’utilisateur** .  
  
    3.  Cliquez sur **Enregistrer**.  
  
## <a name="see-also"></a> Voir aussi  
 [Règles d’entreprise &#40;Master Data Services&#41;](../master-data-services/business-rules-master-data-services.md)   
 [Conditions de règle d’entreprise &#40;Master Data Services&#41;](../master-data-services/business-rule-conditions-master-data-services.md)   
 [Actions de règle d’entreprise &#40;Master Data Services&#41;](../master-data-services/business-rule-actions-master-data-services.md)  
  
  
