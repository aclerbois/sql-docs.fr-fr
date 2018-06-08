---
title: Configurer des règles d’entreprise pour envoyer des notifications (Master Data Services) | Microsoft Docs
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
- business rules [Master Data Services], configuring notifications
- e-mail [Master Data Services], configuring business rules
- notifications [Master Data Services], configuring business rules
ms.assetid: b24f7b11-ab53-4642-999c-e17b543b3558
caps.latest.revision: 10
author: leolimsft
ms.author: lle
manager: craigg
ms.openlocfilehash: f21afcd3b7042c3fc18097fa7f77a604f6e2e926
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="configure-business-rules-to-send-notifications-master-data-services"></a>Configurer des règles d'entreprise pour envoyer des notifications (Master Data Services)

[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

  Dans [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)], configurez des règles d'entreprise pour envoyer des notifications lorsque vous souhaitez informer les utilisateurs des modifications apportées aux valeurs d'attribut.  
  
## <a name="prerequisites"></a>Conditions préalables requises  
 Pour effectuer cette procédure :  
  
-   Vous devez avoir l'autorisation d'accéder aux zones fonctionnelles **Administration de système** et **Autorisations d'accès** . Si vous n'avez pas d'autorisation sur la zone fonctionnelle **Autorisations d'accès** , vous ne pouvez pas afficher la liste des utilisateurs et groupes auxquels envoyer des notifications.  
  
-   Vous devez être administrateur de modèle. Pour plus d’informations, consultez [Administrateurs &#40;Master Data Services&#41;](../master-data-services/administrators-master-data-services.md).  
  
-   Une règle d'entreprise qui utilise une action de validation doit déjà exister. Pour plus d’informations, consultez [Créer et publier une règle d’entreprise &#40;Master Data Services&#41;](../master-data-services/create-and-publish-a-business-rule-master-data-services.md).  
  
-   L’utilisateur ou le groupe qui reçoit la notification doit avoir au moins l’autorisation **Lecture seule** sur l’attribut qui n’a pas été validé. Les utilisateurs ou les groupes qui se voient refuser explicitement ou implicitement l'autorisation à l'attribut recevront le courrier électronique, mais ne seront pas en mesure d'accéder à l'attribut dans [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)].  
  
-   Si un courrier électronique est envoyé à un groupe, seuls les membres du groupe qui ont accédé à [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)] le recevront.  
  
### <a name="to-configure-business-rules-to-send-notifications"></a>Pour configurer des règles d'entreprise pour envoyer des notifications  
  
1.  Dans [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)], cliquez sur **Administration de système**.  
  
2.  Dans la barre de menus, pointez sur **Gérer** et cliquez sur **Règles d'entreprise**.  
  
3.  Sur la page **Règles d’entreprise** , sélectionnez un modèle dans la liste déroulante **Modèle** .  
  
4.  Dans la liste déroulante **Entité** , sélectionnez une entité.  
  
5.  Dans la liste déroulante **Types de membres** , sélectionnez un type de membre.  
  
6.  Dans la grille, sélectionnez la ligne de la règle d’entreprise à modifier, puis cliquez sur **Modifier**.  
  
7.  Cochez la case **Envoyer des notifications** puis, dans la liste déroulante, sélectionnez un utilisateur ou un groupe auxquels envoyer la notification par e-mail.  
  
8.  Cliquez sur **Enregistrer**.  
  
9. Cliquez sur **Tout publier**.  
  
10. Dans la boîte de dialogue de confirmation, cliquez sur **OK**. La valeur de la colonne **État de la règle d’entreprise** est redéfinie sur **Active** , et la colonne **Notification** affiche l’utilisateur ou le groupe auxquels envoyer la notification.  
  
## <a name="next-steps"></a>Next Steps  
  
-   Appliquez des règles d'entreprise aux données en suivant l'une de ces procédures :  
  
    -   [Valider des membres spécifiques par rapport aux règles d’entreprise &#40;Master Data Services&#41;](../master-data-services/validate-specific-members-against-business-rules-master-data-services.md)  
  
    -   [Valider une version par rapport aux règles d’entreprise &#40;Master Data Services&#41;](../master-data-services/validate-a-version-against-business-rules-master-data-services.md)  
  
-   Configurez le protocole de messagerie comme suit :  
  
    -   [Configurer des notifications par e-mail &#40;Master Data Services&#41;](../master-data-services/configure-email-notifications-master-data-services.md)  
  
## <a name="see-also"></a> Voir aussi  
 [Notifications &#40;Master Data Services&#41;](../master-data-services/notifications-master-data-services.md)   
 [Configurer des notifications par courrier électronique &#40;Master Data Services&#41;](../master-data-services/configure-email-notifications-master-data-services.md)  
  
  
