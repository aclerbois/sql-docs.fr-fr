---
title: Définir des autorisations pour des éléments de serveurs de rapports sur un site SharePoint | Microsoft Docs
ms.custom: ''
ms.date: 03/07/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-sharepoint, reporting-services-native
ms.component: security
ms.reviewer: ''
ms.suite: pro-bi
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- permissions [Reporting Services], SharePoint integrated mode
- SharePoint integration [Reporting Services], permissions
ms.assetid: 2467c657-a3bf-4ec3-a88c-8877df19823d
caps.latest.revision: 14
author: markingmyname
ms.author: maghan
manager: kfile
ms.openlocfilehash: fbf0ecedbc5b5b698f32f3c7d5549a604a03e1f7
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="set-permissions-for-report-server-items-on-a-sharepoint-site"></a>Définir les autorisations sur les éléments de serveur de rapports sur un site SharePoint
  Si les paramètres de sécurité par défaut n'offrent pas le niveau d'accès souhaité, vous pouvez créer des niveaux d'autorisation pour fournir un accès aux opérations ou éléments spécifiques d'un serveur de rapports. Les paramètres de sécurité personnalisés peuvent s'avérer utiles si vous souhaitez restreindre l'accès à un rapport particulier.  
  
 Vous devez être propriétaire d'un site pour créer des groupes et des niveaux d'autorisation. Les niveaux d'autorisations sont utilisés globalement dans l'ensemble d'un site. Si vous créez un niveau d'autorisation, celui-ci est accessible aux autres propriétaires de site.  
  
 La plupart des autorisations sont héritées d'un site parent. Si vous attribuez des autorisations à un élément ou à une bibliothèque spécifique, vous rompez l'héritage des autorisations et allourdissez la charge liée à la gestion des autorisations pour cette branche de l'arborescence de votre site.  
  
 Vous pouvez définir des autorisations sur les fichiers de définition de rapport (.rdl), de modèle de rapport (.smdl) et de source de données partagée (.rsds). Vous ne pouvez pas associer sur le même élément des autorisations héritées et des autorisations gérées. Si vous choisissez de gérer directement les autorisations, les autorisations héritées resteront sans effet sur l'élément actif. Pour reprendre ultérieurement l'héritage des autorisations, vous pouvez sélectionner **Hériter les autorisations** dans le menu **Actions** .  
  
 Pour définir des autorisations sur des entités ou des perspectives dans un modèle, vous devez disposer du niveau d'autorisation Contrôle total pour le modèle. Le Contrôle total comprend l'autorisation « Gérer les autorisations », une autorisation de niveau de site accordée à tous les propriétaires de sites et autres groupes SharePoint qui disposent du niveau Contrôle total. Si vous souhaitez accorder à des utilisateurs spécifiques la possibilité de définir la sécurité de l'élément de modèle, vous devez désactiver l'héritage des autorisations et accorder des autorisations de niveau plus élevé (par exemple Contrôle total) à l'utilisateur ou au groupe sur le fichier de modèle. Lorsque vous accordez l'autorisation Contrôle total sur un élément, un fichier dans la bibliothèque, par exemple, les autorisations sont limitées à cet élément et ne s'appliquent pas au parent ou à d'autres éléments de la même bibliothèque. Une fois que l'utilisateur dispose de l'autorisation Gérer les autorisations sur le modèle, il peut définir la sécurité de l'élément de modèle via le site SharePoint ou le Générateur de modèles.  
  
### <a name="to-set-permissions-on-an-individual-report-model-or-data-source"></a>Pour définir des autorisations sur un rapport, un modèle ou une source de données spécifique  
  
1.  Si la bibliothèque n'est pas ouverte, cliquez sur son nom dans le menu de lancement rapide. Si le nom de la bibliothèque n'est pas visible, cliquez sur **Afficher tout le contenu du site**, puis sur le nom de la bibliothèque.  
  
2.  Pointez sur le fichier de rapport, de modèle de rapport ou de source de données partagée.  
  
3.  Cliquez sur la flèche vers le bas et sur **Gérer les autorisations**dans le menu.  
  
4.  Dans le menu **Actions** , cliquez sur **Modifier les autorisations**, puis sur **OK** pour confirmer l'action.  
  
5.  Pour accorder des autorisations à un utilisateur ou un groupe qui ne dispose pas encore d'autorisations pour utiliser le fichier, cliquez sur **Nouveau**, puis sur **Ajouter des utilisateurs**.  
  
6.  Pour supprimer ou modifier des autorisations pour un utilisateur ou à un groupe existant, cliquez sur **Actions**, puis sur **Supprimer les autorisations des utilisateurs** ou **Modifier les autorisations des utilisateurs**.  
  
### <a name="to-set-permissions-that-enable-model-item-security"></a>Pour définir des autorisations qui activent la sécurité des éléments de modèle  
  
1.  Connectez-vous au site SharePoint à l'aide d'un compte qui dispose de l'autorisation Gérer les autorisations sur le site.  
  
2.  Ouvrez la bibliothèque qui contient le modèle.  
  
3.  Pointez sur le modèle.  
  
4.  Cliquez sur la flèche orientée vers le bas en regard du modèle, puis sélectionnez **Gérer les autorisations**.  
  
5.  Cliquez sur **Actions**.  
  
6.  Cliquez sur **Modifier les autorisations**. Cliquez sur **OK**.  
  
7.  Cliquez sur **Nouveau**.  
  
8.  Cliquez sur **Ajouter des utilisateurs**.  
  
9. Dans Utilisateurs/Groupes, entrez le compte d'utilisateur.  
  
10. Sélectionnez **Définir directement les autorisations des utilisateurs**.  
  
11. Cliquez sur **Contrôle total**.  
  
12. Cliquez sur **OK**. Une fois qu'un utilisateur a la possibilité de gérer des autorisations pour un modèle spécifique, celui-ci peut ouvrir le modèle pour modifier les autorisations dans le modèle.  
  
## <a name="see-also"></a> Voir aussi  
 [Utiliser la sécurité intégrée dans Windows SharePoint Services pour les éléments de serveur de rapports](../../reporting-services/security/use-built-in-security-in-windows-sharepoint-services-for-report-server-items.md)   
 [Définir des autorisations pour des opérations de serveurs de rapports dans une application web SharePoint](../../reporting-services/security/set-permissions-for-report-server-operations-in-a-sharepoint-web-application.md)   
 [Comparer des rôles et des tâches dans Reporting Services avec les autorisations et les groupes SharePoint](../../reporting-services/security/reporting-services-roles-tasks-vs-sharepoint-groups-permissions.md)   
 [Article de référence sur les autorisations de site SharePoint et de listes pour les éléments de serveur de rapports](../../reporting-services/security/sharepoint-site-and-list-permission-reference-for-report-server-items.md)   
 [Accord d'autorisations sur des éléments de serveur de rapports sur un site SharePoint](../../reporting-services/security/granting-permissions-on-report-server-items-on-a-sharepoint-site.md)  
  
  
