---
title: Créer, supprimer ou modifier un rôle (Management Studio) | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-sharepoint, reporting-services-native
ms.component: security
ms.reviewer: ''
ms.suite: pro-bi
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- roles [Reporting Services], creating
- deleting roles
- removing roles
- roles [Reporting Services], definitions
- modifying roles
- roles [Reporting Services], deleting
- roles [Reporting Services], modifying
ms.assetid: 3d1d56e6-a283-44a7-8417-36cb4d2c74d1
caps.latest.revision: 42
author: markingmyname
ms.author: maghan
manager: kfile
ms.openlocfilehash: df9601235ca6538447b577154fe629f9e6e6719a
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="role-definitions---create-delete-or-modify"></a>Définitions de rôles - créer, supprimer ou modifier
  Reporting Services fournit des rôles prédéfinis qui établissent un niveau d'accès à un serveur de rapports. Chaque utilisateur ou groupe qui requiert l'accès au serveur de rapports se voit attribuer un rôle qui décrit les tâches pouvant être effectuées. Les rôles sont définis pour l'ensemble du serveur de rapports. Vous ne pouvez pas modifier une définition de rôle pour des parties spécifiques du serveur de rapports, ni spécifier qu'un rôle soit utilisé différemment selon les circonstances.  
  
 Pour créer, modifier ou supprimer des rôles, utilisez [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. Vous ne pouvez supprimer que les rôles qui ne sont pas utilisés.  
  
 Pour attribuer les rôles que vous créez à des utilisateurs et des groupes, utilisez le Gestionnaire de rapports. Pour plus d’informations, consultez [Accorder à un utilisateur l’accès à un serveur de rapports &#40;Gestionnaire de rapports&#41;](../../reporting-services/security/grant-user-access-to-a-report-server-report-manager.md).  
  
> [!NOTE]  
>  Si le serveur de rapports est configuré en mode intégré SharePoint, et si vous vous êtes connecté au site SharePoint auquel le serveur de rapports est intégré, vous pouvez afficher et modifier les niveaux d'autorisation qui contrôlent l'accès au contenu et aux opérations du serveur de rapports.  
  
### <a name="to-create-a-role-definition"></a>Pour créer une définition de rôle  
  
1.  Dans l'Explorateur d'objets, développez un nœud du serveur de rapports.  
  
2.  Développez le dossier Sécurité.  
  
3.  Si vous créez une définition de rôle au niveau d’un élément, cliquez avec le bouton droit sur **Rôles**, puis pointez sur **Nouveau rôle**.  
  
     Ou si vous créez une définition de rôle au niveau système, cliquez avec le bouton droit sur **Rôles système**, puis pointez sur **Nouveau rôle système**.  
  
4.  Tapez un nom unique pour le rôle. Il doit contenir au moins un caractère. Il peut également comprendre des espaces et certains symboles, à l'exception des caractères ; ? : @ & = + , $ / * < > | " ou /.  
  
5.  Tapez éventuellement une description. Dans [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] , cette description n’est visible que dans cette page. Les utilisateurs qui affichent cet élément via le Gestionnaire de rapports peuvent voir la description dans cet outil.  
  
6.  Sélectionnez les tâches pouvant être effectuées par les membres de ce rôle.  
  
7.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
### <a name="to-delete-or-modify-a-role-definition"></a>Pour supprimer ou modifier une définition de rôle  
  
1.  Dans l'Explorateur d'objets, développez un nœud du serveur de rapports.  
  
2.  Développez le dossier Sécurité.  
  
3.  Pour supprimer ou modifier une définition de rôle au niveau d'un élément, développez le dossier Rôles. Effectuez l'une des opérations suivantes :  
  
    1.  Pour supprimer une définition de rôle, cliquez avec le bouton droit sur l’élément, puis cliquez sur **Supprimer**. La boîte de dialogue **Supprimer un objet** s’affiche. [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
    2.  Pour modifier une définition de rôle, cliquez avec le bouton droit sur l’élément, puis cliquez sur **Propriétés**. La page Général de la boîte de dialogue **Propriétés de rôle d’utilisateur** s’affiche.  
  
         Sélectionnez les tâches pouvant être effectuées par les membres de ce rôle, puis cliquez sur **OK**.  
  
4.  Pour supprimer ou modifier une définition de rôle au niveau d’un système, développez le dossier **Rôles système** . Effectuez l'une des opérations suivantes :  
  
    1.  Pour supprimer une définition de rôle système, cliquez avec le bouton droit sur l’élément, puis cliquez sur **Supprimer**. La boîte de dialogue **Supprimer un objet** s’affiche. [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
    2.  Pour modifier une définition de rôle système, cliquez avec le bouton droit sur l’élément, puis cliquez sur **Propriétés**. La page Général de la boîte de dialogue **Propriétés de rôle système** s’affiche.  
  
         Sélectionnez les tâches pouvant être effectuées par les membres de ce rôle, puis cliquez sur **OK** pour appliquer les modifications.  
  
## <a name="see-also"></a> Voir aussi  
 [Se connecter à un serveur de rapports dans Management Studio](../../reporting-services/tools/connect-to-a-report-server-in-management-studio.md)   
 [Créer et gérer des attributions de rôles](../../reporting-services/security/create-and-manage-role-assignments.md)   
 [Reporting Services pour SQL Server Management Studio &#40;SSRS&#41;](../../reporting-services/tools/reporting-services-in-sql-server-management-studio-ssrs.md)  
  
  
